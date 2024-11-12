# TuGraph Analytics交互式图查询：让图所见即所得

TuGraph Analytics提供了OLAP图分析能力，实现图上的交互式查询，用户在构图并导入数据之后，可以通过输入GQL语句对图查询分析，并以可视化的方式直观地展示点边结果。

## OLAP架构

在TuGraph Analytics OLAP架构中，主要以下组件:

- **Client**: 用户通过Client提交查询语句, Client负责和Coordinator交互，发送查询请求。

- **Coordinator**: 接收来自Client查询请求，将查询中的GQL语句进行解析、优化，构建查询的执行计划（执行计划的生成逻辑可参考《分布式图计算如何实现？带你一窥图计算执行计划》[1]），并将任务调度给Worker执行。

- **Worker**: 具体分布式地执行任务的单元，接收到Coordinator发送的Pipeline，执行具体的计算和查询逻辑。

- **Meta Service**: 服务注册管理，Coordinator启动后，会将服务的地址和端口向MetaService进行注册，Client提交查询时从MetaService获取Coordinator的服务地址，进行连接。目前支持http和rpc两种方式。

组件间执行流程如下：

## OLAP流程

## 操作指南

1. **定义图模型**
   
   以下图为例，图中有2种点person和software，以及2种边knows和creates。

   ![图模型](#)

   图模型定义可参考《TuGraph Analytics图建模研发：为图计算业务提速增效》[2]，图定义语法为：

   ```sql
   CREATE GRAPH dy_modern (
       Vertex person (
         id bigint ID,
         name varchar,
         age int
       ),
       Vertex software (
         id bigint ID,
         name varchar,
         lang varchar
       ),
       Edge knows (
         srcId bigint SOURCE ID,
         targetId bigint DESTINATION ID,
         weight int
       ),
       Edge creates (
         srcId bigint SOURCE ID,
         targetId bigint DESTINATION ID,
         weight int
       )
   ) WITH (
       storeType='rocksdb',
       shardCount = 2
   );
   ```

2. **准备图数据**
   
   创建“加工”类型图任务，发布生成图作业。

   ```sql
   USE GRAPH dy_modern;

   INSERT INTO dy_modern.person(id, name, age)
   SELECT 1, 'jim', 20
   UNION ALL
   SELECT 2, 'kate', 22
   UNION ALL
   SELECT 3, 'tom', 24;

   INSERT INTO dy_modern.software(id, name, lang)
   SELECT 4, 'software1', 'java'
   UNION ALL
   SELECT 5, 'software2', 'java';

   INSERT INTO dy_modern.knows
   SELECT 1,2,2
   UNION ALL
   SELECT 1,3,3
   UNION ALL
   SELECT 3,2,3;

   INSERT INTO dy_modern.creates
   SELECT 2,4,6
   UNION ALL
   SELECT 3,5,8
   UNION ALL
   SELECT 3,4,8;
   ```

   图作业需要的worker数为23，在作业界面将参数进行修改，之后提交作业运行。

3. **创建查询服务**
   
   创建图查询服务, 任务类型选择“图查询”，目标图选择刚才创建的图。

   发布任务后，使用默认参数即可，提交作业。

4. **执行查询**
   
   图查询服务的作业变成RUNNING状态后，可在任务界面点击“查询”进入图查询界面。

   输入相应的gql查询语句，点击“执行”，即可得到查询结果。

5. **图可视化**
   
   点击某个点，可以查看点关联的具体信息和属性，以及关联的其他点边。

   除了可视化的方式，也可以json形式看到返回的结果。

至此，我们就成功使用TuGraph Analytics实现了图上的交互式查询！是不是超简单！快来试一试吧！



