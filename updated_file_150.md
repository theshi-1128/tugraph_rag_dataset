# TuGraph开源JAVA客户端工具 TuGraph-OGM

## 无缝对接JAVA开发生态

TuGraph 图数据库提供了 JAVA、C++、Python 等多种语言的 SDK 支持，方便客户在各种场景下使用。用户使用 SDK 向 TuGraph 服务器发送 Cypher 请求，服务器则以 JSON 形式返回数据。近日，TuGraph 推出了一款面向 JAVA 客户端用户的开发工具 TuGraph-OGM (Object Graph Mapping)，为用户提供了对象操作接口，相较于 Cypher/JSON 接口应用起来更加便捷。

OGM 类似于关系数据库中的 ORM（Object Relational Model），可以将数据库返回的数据自动映射成 JAVA 中的对象，方便用户读取。而用户对这些对象的更新操作也可以被自动翻译成 Cypher 语句发送给服务器。这样，即便是完全不懂 Cypher 的用户，也可以通过操作对象与数据库进行交互，大大降低了图数据库的使用门槛。

TuGraph-OGM 同时也兼容其他开源产品 OGM 工具如 Neo4j-OGM，方便用户将工程在不同数据库与 TuGraph 数据库间无缝迁移。本文将对 TuGraph-OGM 进行全面的介绍。

## 0 映射原理

TuGraph-OGM 将 JAVA 对象映射为图的对象，类映射为点，类的属性映射为图中的属性，类中的方法映射为操作 TuGraph 的查询语句。

以电影场景为例，对演员、电影、导演之间的关系进行数据化，就形成了非常典型的图数据。举一个简单的示例，演员 Alice 在 1990 年和 2019 年分别出演了两部电影《Jokes》和《Speed》，其中《Jokes》的导演是 Frank Darabont。以图的思维来看，演员、导演、电影可以被映射为三种不同的节点，而出演、执导可以被映射为两种边，映射结果如上图所示。将数据存入图数据库后，相关的开发人员就可以使用各类图查询语言对数据进行查询。但对非图数据库相关的开发人员来说，这个例子中的演员、导演、电影作为实体，同样可以映射为类中的对象，而与实体相关联的对象可以通过集合存储，这是大多数开发人员熟悉的领域。

## 1 TuGraph-OGM架构

TuGraph-OGM 可被看做一个 "翻译器"，主要功能是将开发人员对 JAVA 对象的一些操作翻译为 TuGraph 可理解的图查询语言 Cypher 请求，并将该操作返回的结果，再次翻译为 JAVA 对象。架构图如下所示：

## 3 使用示例

详细示例请参考 tugraph-ogm-integration-tests 在 pom.xml 中引入依赖。

### 3.1 构建图对象

首先需要通过注解标明图中的实体。

- `@NodeEntity`：该注解标明的类为节点类。
- `@Relationship`：用于标明边，同时 @Relationship 中可指定 label 与边的指向。
- `@Id`：用于标明 identity，是 OGM 中数据的唯一标识。

### 3.2 与 TuGraph 建立连接

当前 TuGraph-OGM 提供了 RPC driver 用于连接 TuGraph，具体配置如下所示：

### 3.3 通过 OGM 进行增删改查

OGM 支持对 TuGraph 的实体执行 CRUD 操作，同时支持发送任意 TuGraph 支持的 Cypher 语句，包括通过 CALL 调用存储过程。

#### CREATE

在完成图对象的构建后，即可通过类的实例化创建节点。当两个节点互相存储在对方的集合（该集合在构建时被标注为边）中，就形成了一条边。最后使用 `session.save` 方法将数据存入数据库。注意：TuGraph 数据库为强 schema 类型数据库，在创建实体前需要该数据的 label 已经存在，且新建过程中需要提供唯一的主键。

#### DELETE

使用 `session.delete` 方法删除节点，同时会删除与节点相关联的所有边。

#### UPDATE

修改已创建的节点的属性，再次调用 `session.save` 方法会对节点进行更新。

#### MATCH

`session.load` 方法用于根据节点 ID 查找节点。`session.loadAll` 方法用于批量查找节点，支持通过多个节点 ID 查找节点、查找某一类型的所有节点、带有 filter 的查询。filter 查询需要新建 Filter，传入参数 ComparisonOperator，可选为：EQUALS、GREATER_THAN、LESS_THAN。

#### QUERY WITH CYPHER

OGM 支持通过 `queryForObject`、`query` 方法向 TuGraph 发送 Cypher 查询，由于 Cypher 查询的灵活性，需要用户自行指定返回结果格式。

- `session.queryForObject` 方法：需要在方法第一个参数处指定返回类型，可设定为某一实体类或数字类型。
- `session.query` 方法：Cypher 查询的返回结果被存储为 Result 类型，其内部数据需要用户自行解析。以下方代码为例，传入数据库的 Cypher 为 CREATE 查询，返回结果 `createResult` 可被解析为 `QueryStatistics`，可获取到此次查询被创建的节点与边的数目。


