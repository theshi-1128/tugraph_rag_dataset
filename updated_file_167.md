# 源码解读｜TuGraph key-val存储

## 存储模型

TuGraph存储层采用开源的lmdb实现，底层存储采用了B+树，在B+树之上抽象出了一层key-val存储。它与基于hash的key-val存储有着本质的区别，基于hash的key-val存储在无hash碰撞的情况下，一次key-val的查询只需要一次hash计算和一次内存寻址，时间复杂度是O(1)，但在TuGraph的实现中，key-val层是基于B+树来实现的，key-val查找的本质还是基于B+树的范围查找，因此一次key-val的查询是要在树中进行二分查找的，性能是O(log(n))。

TuGraph基于lmdb的存储模型，对由点边数据组成的key-val进行了包装，将点边数据打包保存在一起，对于非大点，通过一次key-val的查询就可以查找到点以及该点的出边和入边，减少了在B+树中进行二分查找的次数，将基于全图完整数据的查找限定在某个key对应的value中，提升了TuGraph在适用场景下的查找性能。对于大点来说，为了避免某个key对应的value过大，影响写入或修改性能，权衡之下将点和边数据以阀值按类型切分为多个value，分别保存在不同的类型的key中，既提升了查找性能，也降低了大点带来的写入性能损失。

## key的类型

```cpp
enum PackType {
    PACKED_DATA = 0, 
    VERTEX_ONLY = 1, 
    OUT_EDGE = 2,    
    IN_EDGE = 3     
};
```

### PACKED_DATA

PACKED_DATA类型的key共计6字节，由5字节的点id和1字节的类型组成，此类key对应的是混合存储的val。将某个点和与之关联的边数据打包在一起保存，适用于非大点的存储，新插入的点也属于此类型。

### VERTEX_ONLY

VERTEX_ONLY类型key共6字节，由5字节的点id和1字节的类型组成，此类key对应的val中只保存了单点的属性，当一个PACKED_DATA类型的key对应的value长度超过阀值::lgraph::_detail::NODE_SPLIT_THRESHOLD(1000字节)时，会被拆分成VERTEX_ONLY，OUT_EDGE，IN_EDGE三种类型，分别表示一个单独的点，一组从该点出发的边，和一组目标点为该点的边。

### OUT_EDGE

OUT_EDGE类型key共计25字节，由5字节的源点id，1字节的类型，2字节的边label id，8字节的tid，5字节的目标点id，以及4字节的eid组成。此类key对应的val中保存了由src_vertex_id点出发，按label_id和tid以及目标点id排序好的一组出边。适用于大点切分后保存出边。其中key中的源点id和类型对于value中的所有值都是一致的，label id，tid以及目标点id是取了这组边中最后一个边的属性。

### IN_EDGE

IN_EDGE类型key共计25字节，由5字节的目标点id，1字节的类型，2字节的边label id，8字节的tid，5字节的最大源点id，以及4字节的eid组成，此类key对应的val中保存了指向dst_vertex_id，按label_id和tid以及源点id排序好的一组出边。适用于大点切分后保存出边。其中key中的目标点id和类型对于value中的所有值都是一致的，label id，tid以及目标点id是取了这组边中最后一个边的属性。

## 获取key中对应的属性

可以通过key的地址加上对应属性的字节偏移获取对应的值。

```cpp
static const size_t FID_OFF = 0;                  // first vid  0
static const size_t PT_OFF = FID_OFF + 5;          // pair type  5
static const size_t LID_OFF = PT_OFF + 1;         // label      6
static const size_t TID_OFF = LID_OFF + 2;        // primary id 8
static const size_t SID_OFF = TID_OFF + 8;        // second vid 16
static const size_t EID_OFF = SID_OFF + 5;        // edge id    21
static const size_t EDGE_KEY_SIZE = EID_OFF + 4;  //      25
```

## val的类型

### VertexValue

VertexValue类型的值用于保存key为VERTEX_ONLY的值，代表单个的点数据，数据格式按照预定义好的schema来保存，存储格式如下：

### EdgeValue

EdgeValue类型的值用于保存key为OUT_EDGE与IN_EDGE的值，代表从某个点出发，或者指向某个点的一组边数据。存储格式如下，绿色部分是EdgeValue，它包含三部分内容，第一部分是1字节的边个数，因此一个EdgeValue类型的数据中最多只能保存255条边，第二部分是一个偏移数组，代表了每条边在value中的偏移位置，第三部分是边的内容数组，每个成员代表了一条边。

边的内容包含edge header和edge value两部分。edge header由一字节的指示器开始，它的作用是标识后面紧跟着的几个字段长度。edge value保存的是按照edge schema序列化好的数据。这里为了节省磁盘空间，对edge_label_id, temporal_id, vertex_id以及edge_id进行了压缩，采用的压缩算法如下：

- **edge_label_id**：如果edge_label_id为0则不占用存储空间，如果edge_label_id < 0x100(十进制256)则占用1字节，如果0x100 <= edge_label_id < 0xFF则占用两字节。
- **temporal_id**：如果temporal_id为0则不占用存储空间，不为0则占用8字节存储空间。
- **vertex_id**：去除vertex_id中前导的0，按8位对齐。
- **edge_id**：去除edge_id中前导的0，按8位对齐。

### PackedDataValue

PackedDataValue类型的值用于保存key为PACKED_DATA的value，代表将某个点与其出边、入边一起打包后保存，因为将点和边打包在同一段存储结构中，因此结构复杂，它包含两个int32_t类型的偏移，分别指向这个点的出边和入边的首地址，以及一个VertexValue表示一个点和两个EdgeValue，分别表示这个点的出边和入边。存储格式如下，其中红色部分是PackedDataValue，绿色部分是EdgeValue，紫色部分是VertexValue（保存的是按照vertex schema序列化好的点数据），蓝色部分是按照edge schema序列化好的边数据。


