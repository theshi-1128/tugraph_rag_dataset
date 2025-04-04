# TuGraph-OGM

## 1.简介

> TuGraph-OGM 项目在其他仓库开源。

TuGraph-OGM(Object Graph Mapping)为面向 TuGraph 的图对象映射工具，支持将 JAVA 对象（POJO）映射到 TuGraph 中，JAVA 中的类映射为图中的节点、类中的集合映射为边、类的属性映射为图对象的属性，并提供了对应的函数操作图数据库，因此 JAVA 开发人员可以在熟悉的生态中轻松地使用 TuGraph 数据库。同时 TuGraph-OGM 兼容 Neo4j-OGM，Neo4j 生态用户可以无缝迁移到 TuGraph 数据库上。

### 1.1.TuGraph-OGM 功能

TuGraph-OGM 提供以下函数操作 TuGraph：

| 功能                            | 用法                                                                             |
| ------------------------------- | -------------------------------------------------------------------------------- |
| 插入单个节点\边                 | void session.save(T object)                                                      |
| 批量插入节点\边                 | void session.save(T object)                                                      |
| 删除节点与对应边                | void session.delete(T object)                                                    |
| 删除指定 label 的全部节点       | void session.deleteAll(Class\<T> type)                                           |
| 清空数据库                      | void purgeDatabase()                                                             |
| 更新节点                        | void session.save(T newObject)                                                   |
| 根据 id 查询单个节点            | T load(Class<T> type, ID id)                                                     |
| 根据 ids 查询多个节点           | Collection\<T> loadAll(Class\<T> type, Collection<ID> ids)                       |
| 根据 label 查询全部节点         | Collection\<T> loadAll(Class\<T> type)                                           |
| 条件查询                        | Collection\<T> loadAll(Class\<T> type, Filters filters)                          |
| Cypher 查询（指定返回结果类型） | T queryForObject(Class\<T> objectType, String cypher, Map<String, ?> parameters) |
| Cypher 查询                     | Result query(String cypher, Map<String, ?> parameters)                           |
表格内容描述:

该表格列出了不同的功能及其对应的用法。具体列名为“功能”和“用法”。

1. 插入单个节点或边的功能使用`void session.save(T object)`方法。
2. 批量插入节点或边采用与单个插入相同的方法，即`void session.save(T object)`。
3. 删除节点及其对应边的功能可以通过`void session.delete(T object)`实现。
4. 如果需要删除指定label的全部节点，可使用`void session.deleteAll(Class<T> type)`方法。
5. 清空数据库的操作使用`void purgeDatabase()`方法。
6. 更新节点的功能可通过`void session.save(T newObject)`实现。
7. 查询单个节点根据id使用`T load(Class<T> type, ID id)`方法。
8. 根据一组ids查询多个节点，可使用`Collection<T> loadAll(Class<T> type, Collection<ID> ids)`。
9. 查询所有指定label的节点，使用`Collection<T> loadAll(Class<T> type)`。
10. 条件查询功能通过`Collection<T> loadAll(Class<T> type, Filters filters)`来实现。
11. 使用Cypher查询时，可以指定返回的结果类型，通过`T queryForObject(Class<T> objectType, String cypher, Map<String, ?> parameters)`。
12. 进行通用的Cypher查询，则使用`Result query(String cypher, Map<String, ?> parameters)`。

综上所述，该表格提供了一系列与数据库操作相关的功能及其具体实现方法，涵盖了节点和边的插入、删除、更新、查询等基本操作，适用于开发人员在使用数据库时的参考。

## 2.编译 TuGraph-OGM

```shell
cd tugraph-ogm
mvn clean install -DskipTests -Denforcer.skip=true
```

## 3.使用 TuGraph-OGM

详细示例请参考 demo 文件夹下的 TuGraphOGMDemo ###在`pom.xml`中引入依赖

```
<dependency>
        <groupId>org.neo4j</groupId>
        <artifactId>neo4j-ogm-api</artifactId>
        <version>0.1.0-SNAPSHOT</version>
    </dependency>

    <dependency>
        <groupId>org.neo4j</groupId>
        <artifactId>neo4j-ogm-core</artifactId>
        <version>0.1.0-SNAPSHOT</version>
    </dependency>

    <dependency>
        <groupId>org.neo4j</groupId>
        <artifactId>tugraph-rpc-driver</artifactId>
        <version>0.1.0-SNAPSHOT</version>
    </dependency>
```

### 3.1.构建图对象

```java
@NodeEntity
public class Movie {      // 构建Movie节点
    @Id
    private Long id;      // Movie节点的id
    private String title; // title属性
    private int released; // released属性

    // 构建边ACTS_IN    (actor)-[:ACTS_IN]->(movie)
    @Relationship(type = "ACTS_IN", direction = Relationship.Direction.INCOMING)
    Set<Actor> actors = new HashSet<>();

    public Movie(String title, int year) {
        this.title = title;
        this.released = year;
    }

    public Long getId() {
        return id;
    }

    public void setReleased(int released) {
        this.released = released;
    }
}

@NodeEntity
public class Actor {      // 构建Actor节点
    @Id
    private Long id;
    private String name;

    @Relationship(type = "ACTS_IN", direction = Relationship.Direction.OUTGOING)
    private Set<Movie> movies = new HashSet<>();

    public Actor(String name) {
        this.name = name;
    }

    public void actsIn(Movie movie) {
        movies.add(movie);
        movie.getActors().add(this);
    }
}
```

### 3.2.与TuGraph建立连接

```java
// 配置
String databaseUri = "list://ip:port";
String username = "admin";
String password = "73@TuGraph";
//启动driver
Driver driver = new RpcDriver();
Configuration.Builder baseConfigurationBuilder = new Configuration.Builder()
                            .uri(databaseUri)
                            .verifyConnection(true)
                            .credentials(username, password);
                            driver.configure(baseConfigurationBuilder.build());
driver.configure(baseConfigurationBuilder.build());
// 开启session
SessionFactory sessionFactory = new SessionFactory(driver, "entity_path");
Session session = sessionFactory.openSession();
```

### 3.3.通过OGM进行增删改查

```java
// 增
Movie jokes = new Movie("Jokes", 1990);  // 新建Movie节点jokes
session.save(jokes);                     // 将jokes存储在TuGraph中

Movie speed = new Movie("Speed", 2019);
Actor alice = new Actor("Alice Neeves");
alice.actsIn(speed);                    // 将speed节点与alice节点通过ACTS_IN进行连接
session.save(speed);                    // 存储两个节点与一条边

// 删
session.delete(alice);                  // 删除alice节点以及相连的边
Movie m = session.load(Movie.class, jokes.getId());   // 根据jokes节点的id获取jokes节点
session.delete(m);                                    // 删除jokes节点

// 改
speed.setReleased(2018);
session.save(speed);                   // 更新speed节点属性

// 查
Collection<Movie> movies = session.loadAll(Movie.class);  // 获取所有Movie节点
Collection<Movie> moviesFilter = session.loadAll(Movie.class,
        new Filter("released", ComparisonOperator.LESS_THAN, 1995));  // 查询所有小于1995年发布的电影

// 调用Cypher
HashMap<String, Object> parameters = new HashMap<>();
parameters.put("Speed", 2018);
Movie cm = session.queryForObject(Movie.class,
        "MATCH (cm:Movie{Speed: $Speed}) RETURN *", parameters);      // 查询Speed为2018的Movie

session.query("CALL db.createVertexLabel('Director', 'name', 'name'," +
        "STRING, false, 'age', INT16, true)", emptyMap());            // 创建节点Label Director
session.query("CALL db.createEdgeLabel('DIRECT', '[]')", emptyMap()); // 创建边Label DIRECT
Result createResult = session.query(
        "CREATE (n:Movie{title:\"The Shawshank Redemption\", released:1994})" +
        "<-[r:DIRECT]-" +
        "(n2:Director{name:\"Frank Darabont\", age:63})",
        emptyMap());
QueryStatistics statistics = createResult.queryStatistics();          // 获取create结果
System.out.println("created " + statistics.getNodesCreated() + " vertices");    // 查看创建节点数目
System.out.println("created " + statistics.getRelationshipsCreated() + " edges");  //查看创建边数目

// 清空数据库
session.deleteAll(Movie.class);        // 删除所有Movie节点
session.purgeDatabase();               // 删除全部数据
```
