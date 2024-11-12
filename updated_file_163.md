# 社区贡献 | 新Feature发布：TuGraph-DB支持空间数据类型

本文介绍了来自北京大学的TuGraph开源社区开发者孙同学的工作，孙同学基于开源图数据库TuGraph-DB的开发，支持了空间数据类型。文章将从空间数据类型的应用、在TuGraph-DB中的实现以及使用三个方面进行介绍。孙同学的工作丰富了TuGraph-DB功能，因此获得“TuGraph社区年度优秀贡献者”，再次感谢孙同学的贡献，项目功能正在持续完善中，也欢迎更多伙伴加入TuGraph社区一起共建。

近年来，地理数据类型/空间数据类型（Spatial Data Type）在图数据库中的应用价值显著，它不仅增强了数据的表达能力，还促进了跨领域数据的融合分析，尤其在社交网络、地图探索、城市规划等关键领域展现了强大的实用价值。

## 空间数据类型在图数据库中的应用案例

空间地理数据也是天然适合使用图数据库的一种场景，我们先来看看在图数据库中使用空间数据的几个经典场景:

### 场景案例一：判断某空间类型内的坐标

如上图1和图2所示，日常生活中我们通常需要在地图中查询距离自己一定距离内的景点/美食信息。对应的在图数据库中，即为判断哪些坐标在以某点为中心的圆形或矩形区域内，对应的Cypher查询语句如下:

#### 判断在圆形区域内

```cypher
WITH point({latitude: $latitude, longitude:$longitude}) AS radiusCenter

MATCH (p:Point)-[:HAS_GEOMETRY]-(poi:PointOfInterest)-[:HAS_TAGS]->(t:Tags)
    WHERE point.distance(p.location, radiusCenter) < $radius
RETURN p {
    latitude: p.location.latitude,
    longitude: p.location.longitude,
    name: poi.name,
    categories: labels(poi),
    tags: t{.*}                       
} AS point
```

#### 判断在矩形区域内

```cypher
MATCH (p:Point)-[:HAS_GEOMETRY]-(poi:PointOfInterest)-[:HAS_TAGS]->(t:Tags)
   WHERE point.withinBBox(
        p.location,
        point({longitude: $lowerLeftLon, latitude: $lowerLeftLat}),
        point({longitude: $upperRightLon, latitude: $upperRightLat})
   )
RETURN p {
  latitude: p.location.latitude,
  longitude: p.location.longitude,
  name: poi.name,
  categories: labels(poi),
  tags: t{.*}
} AS point
```

### 场景案例二：判断路径与某空间类型是否重合

如上图3所示，移动物体的行动轨迹可以被抽象成一条线，这条轨迹往往伴随着时序信息。日常生活中，有时候我们想知道哪部分行动轨迹和给定区域有重合。对应在图数据库中，即判断一系列坐标中哪些坐标落在在空间类型内。对应的Cypher查询语句如下:

```cypher
WITH point({latitude: $latitude, longitude: $longitude}) AS radiusCenter

MATCH (g: Geometry)
    WHERE any(
        p IN g.coordiates WHERE point.distance(p, radiusCenter) < $radius
    )
RETURN [n IN g.coordinates | [n.latitude, n.longitude]] AS route
```

## 空间数据类型在TuGraph-DB中的实现

### 需求分析

结合上述案例，我们可以分析总结出对空间数据类型的需求:

- 支持不同坐标系下（包括地球地理坐标系，平面几何坐标系等）不同空间数据类型（包括Point、LineString、Polygon）的存储与创建
- 支持不同坐标系下的常见空间查询操作, 包括Distance、BoundingBox、Disjoint（判断两个数据是否相交）的查询等
- 支持空间数据索引（R-Tree）
- 支持常见空间数据格式的导入（ESRI Shapefile data / OpenStreetMap）
- 支持空间数据的可视化

### 空间数据类型的表示

空间数据类型可以用不同的坐标系来表示，EPSG[1]是一个标准化的地理空间参考系统标识符集合，用于标识不同的地理空间参考系统，包括坐标系统、地理坐标系、投影坐标系等。通常使用EPSG编码表示数据的坐标系。行业内一般采用

- WGS84坐标系（没错，就是GPS系统的坐标系），标识符为EPSG 4326
- Cartesian（笛卡尔）坐标系（没错，就是你高中数学学的直角坐标系），标识符为EPSG 7203

WGS84是全球定位系统(GPS)的基础，允许全球的GPS接收器确定精确位置。几乎所有现代GPS设备都是基于WGS84坐标系来提供位置信息。在地图制作和GIS（地图制作和地理信息系统）领域，WGS84被广泛用于定义地球上的位置。这包括各种类型的地图创建、空间数据分析和管理等。

Cartesian（笛卡尔）坐标系，又称直角坐标系，是一种最基本、最广泛应用的坐标系统。它通过两条数轴定义一个平面，三条数轴定义一个空间，这些轴互相垂直，在数学、物理、工程、天文和许多其他领域中有着广泛的应用。

### 空间数据类型的实现

OGC(Open Geospatial Consortium) 定义了空间数据的标准表示格式，分别为EWKT(extended well known text)与EWKB(extended well known binary)格式，用于在不同系统和平台之间交换和存储空间数据，现已被广泛采用。

#### EWKT

EWKT格式数据如下所示，先指定空间数据类型，再在括号内指定具体的坐标，一个坐标对表示一个点，每个坐标对之间用逗号隔开。其中，对于Polygon类型的数据，第一个坐标对需要与最后一个坐标对相同，形成闭合的面。

```
SRID=s;POINT (<x> <y>)
SRID=s;LINESTRING(<x1> <y1>, <x2> <y2>, …)
```
注: SRID默认为4326, 可以不指定

#### EWKB

EWKB格式数据如下图所示，

- 第0-1位: 表示编码方式 00表示大端法，01表示小端法
- 第2 - 5位: 空间数据类型
  - 0100: point
  - 0200: linestring
  - 0300: polygon
- 第6 - 9位: 数据维度
  - 0020: 二维
  - 0030: 三维
- 第10 - 17位: 坐标系的EPSG编码
- 第18 - n位: double类型的坐标对的16进制表示

注: 对于POINT类型，其EWKB格式为定长存储，固定长度为50，而对于其他类型，则为不定长。

### 实现思路

在TuGraph-DB的实现，基于boost geometry库的基础上进行封装，用EWKB格式存储数据，其中Point类型为定长存储50，其余皆为变长存储。我们支持了Point, Linestring与Polygon三种类型，同时支持了WGS84, CARTESIAN两种坐标系，数据类型与坐标系均可根据需要拓展。

## 在TuGraph-DB中使用空间数据类型

### 定义空间数据类型

TuGraph-DB当前已经支持Point、Linestring与Polygon三种类型

- Point：点，创建方式例如POINT(2.0, 2.0, 7203)
- Linestring：折线，创建方式例如LINESTRING(0 2,1 1,2 0)
- Polygon：多边形，创建方式例如POLYGON((0 0,0 7,4 2,2 0,0 0))

其中坐标点都是double型

### 相关函数介绍

创建空间数据相关函数，以Point为例：

| 函数名      | 描述                              | 输入参数                                 | 返回值类型 |
|-------------|-----------------------------------|------------------------------------------|------------|
| `Point()`   | 根据坐标或EWKB创建Point            | 坐标对 (double, double) / EWKB format(string) | point      |
| `PointWKB()`| 根据WKB与指定SRID创建Point         | WKB format(string), SRID(int)            | point      |
| `PointWKT()`| 根据WKT与指定SRID创建Point         | WKT format(string), SRID(int)            | point      |
表格内容描述：

该表格包含四列，分别为“函数名”、“描述”、“输入参数”和“返回值类型”。每一行对应一个函数的详细信息。

1. 第一行涉及的函数是 `Point()`，它的功能是根据给定的坐标对或EWKB格式创建一个Point对象。输入参数可以是一个双精度浮点数的坐标对或者一个EWKB格式的字符串，返回值的类型为Point。

2. 第二行的函数是 `PointWKB()`，该函数用于根据WKB格式和指定的SRID创建一个Point对象。它接受的输入参数是一个WKB格式的字符串和一个整型SRID，返回值的类型同样是Point。

3. 第三行介绍的函数是 `PointWKT()`，其功能是根据WKT格式和指定的SRID创建Point对象。输入参数包括一个WKT格式的字符串和一个整型SRID，返回值的类型为Point。

总结：该表格展示了三个用于创建Point对象的函数，并详细说明了每个函数的描述、输入参数及返回值类型。所有函数均返回Point类型的对象，输入参数涵盖了不同格式的数据。


### 查询用相关函数：

| 函数名      | 描述                              | 输入参数                                 | 返回值类型 |
|-------------|-----------------------------------|------------------------------------------|------------|
| `Distance()`   | 计算两个空间数据间的距离	           | 注：要求坐标系相同| Spatial data1, Spatial data2         | double	    |
| `Disjoint()`| 判断两个空间数据是否相交	         | 注：开发中| Spatial data1, Spatial data2        | bool	    |
| `WithinBBox()`| 判断某个空间数据是否在给定的长方形区域内	       | 注：开发中| Spatial data, Point1       | bool	    |
表格内容描述：
该表格包含四个列名：函数名、描述、输入参数和返回值类型。

1. `Distance()`函数用于计算两个空间数据之间的距离。该函数要求输入的空间数据具有相同的坐标系，输入参数为两个空间数据（Spatial data1和Spatial data2），返回值类型为一个双精度浮点数（double）。

2. `Disjoint()`函数用于判断两个空间数据是否相交。该函数目前处于开发中，输入参数同样为两个空间数据（Spatial data1和Spatial data2），返回值类型为布尔值（bool），用于表示相交状态。

3. `WithinBBox()`函数用于判断某个空间数据是否位于指定的长方形区域内。该函数也处于开发中，输入参数包括一个空间数据（Spatial data）和一个点（Point1），返回值类型为布尔值（bool），用于表明该空间数据是否在指定区域内。

整体总结：该表格列出了三种空间数据处理的函数，它们的功能包括计算距离、判断相交与判断是否在指定区域内。其中，`Distance()`已完成，而`Disjoint()`和`WithinBBox()`仍在开发中。返回值类型涵盖了双精度浮点数和布尔值两种。

### 使用实例如下：

#### 创建包含空间数据类型的点模型

CALL db.createVertexLabel('food', 'id', 'id', int64, false, 'name', string, true,'pointTest',point,true) 

#### 插入标记美食点的数据

CREATE (n:food {id:10001, name: 'aco Bell',pointTest:point(3.0,4.0,7203)}) RETURN n

#### 创建具有折线属性的点模型

CALL db.createVertexLabel('lineTest', 'id', 'id', int64, false, 'name', string, true,'linestringTest',linestring,true)

#### 插入具有折线属性的点数据

CREATE (n:lineTest {id:102, name: 'Tom',linestringTest:linestringwkt('LINESTRING(0 2,1 1,2 0)', 7203)}) RETURN n

#### 创建具有多边型属性的点模型

CALL db.createVertexLabel('polygonTest', 'id', 'id', int64, false, 'name', string, true,'polygonTest',polygon,true)

#### 插入具有多边型属性的点数据

CREATE (n:polygonTest {id:103, name: 'polygonTest',polygonTest:polygonwkt('POLYGON((0 0,0 7,4 2,2 0,0 0))', 7203)}) RETURN n

## Demo演示：基于地理位置个性化推荐的美食探索

在本章节中，我们将探索如何利用TuGraph-DB图数据库的地理空间功能，构建一个生动有趣的美食探索应用，“人”与“美食”通过地理位置紧密相连，实现个性化美食推荐。

### 数据模型设计

定义两种核心节点类型：

- **Food（美食）点**：每一家餐厅或小吃店都可以作为一个Food节点，其属性包括但不限于名称、地址、评分、美食类别等。特别地，我们将在每个Food节点上附加地理坐标信息，用以精确记录其地理位置。

- **Person（人物）点**：代表应用的用户，属性包含用户名、当前位置等。用户的当前位置同样通过地理坐标表示，便于后续的地理空间查询。

### 创建Schema

CALL db.createVertexLabel('food', 'id', 'id', int64, false, 'name', string, true,'pointTest',point,true,'mark',double,true)

CALL db.createVertexLabel('person', 'id', 'id', int64, false, 'name', string, true,'pointTest',point,true)

### 插入示例数据 food

CREATE (n:food {id:10001, name: 'Starbucks',pointTest:point(1.0,1.0,7203),mark:4.8}) RETURN n

CREATE (n:food {id:10002, name: 'KFC',pointTest:point(2.0,1.0,7203),mark:4.5}) RETURN n

CREATE (n:food {id:10003, name: 'Pizza Hut',pointTest:point(2.0,5.0,7203),mark:4.5}) RETURN n

CREATE (n:food {id:10004, name: 'Taco Bell',pointTest:point(3.0,4.0,7203),mark:4.7}) RETURN n

CREATE (n:food {id:10005, name: 'Pizza Fusion',pointTest:point(5.0,3.0,7203),mark:4.9}) RETURN n

CREATE (n:food {id:10006, name: 'HaiDiLao Hot Pot',pointTest:point(2.0,2.0,7203),mark:4.8}) RETURN n

CREATE (n:food {id:10007, name: 'Lao Sze Chuan',pointTest:point(4.0,3.0,7203),mark:4.7}) RETURN n

### 插入示例数据 person

CREATE (n:person {id:1, name: 'Tom',pointTest:point(3.0,3.0,7203)}) RETURN n

### 构建美食探索查询

能够根据用户的当前位置，寻找距离2.5以内的美食，根据距离进行升序排列，返回距离和评分。

MATCH (n:person{id:1}), (m:food)

WITH n.pointTest AS p1, m.pointTest AS p2, m.name AS food, m.mark AS mark

CALL spatial.distance(p1,p2) YIELD distance 

WHERE distance < 2.5

RETURN food,distance,mark

ORDER BY distance

此查询首先匹配特定的Person节点，然后找到所有Food节点，利用自定义的distance函数计算Person节点当前位置与每个Food节点之间的直线距离，筛选出距离在2.5之内的美食。最后，按照美食的距离升序排列结果，附带评分参考，为用户提供最优质的推荐。

### 未来展望

孙同学的工作，让TuGraph-DB具备了处理地理空间数据的能力。未来，TuGraph-DB将来会继续实现Disjoint() 、WithinBBox()等更多的函数，以及实现更高级的索引、数据导入、可视化等功能，丰富更多的使用场景。作为开源项目，TuGraph社区也欢迎大家一起参与共建开发地理空间功能。


