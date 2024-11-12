# JDBC Connector介绍
JDBC Connector由社区贡献，支持读和写。
## 语法

```sql
CREATE TABLE jdbc_table (
  id BIGINT,
  name VARCHAR,
  age INT
) WITH (
	type='jdbc',
    geaflow.dsl.jdbc.driver = 'org.h2.Driver',
    geaflow.dsl.jdbc.url = 'jdbc:h2:mem:testdb;DB_CLOSE_DELAY=-1',
    geaflow.dsl.jdbc.username = 'h2_user',
    geaflow.dsl.jdbc.password = 'h2_pwd',
    geaflow.dsl.jdbc.table.name = 'source_table'
);
```
## 参数

| 参数名 | 是否必须 | 描述                                                |
| -------- |------|---------------------------------------------------|
| geaflow.dsl.jdbc.driver     | 是    | The JDBC driver.                                  |
| geaflow.dsl.jdbc.url     | 是    | The database URL.                                 |
| geaflow.dsl.jdbc.username     | 是    | The database username.                            |
| geaflow.dsl.jdbc.password     | 是    | The database password.                            |
| geaflow.dsl.jdbc.table.name     | 是    | The table name.                                   |
| geaflow.dsl.jdbc.partition.num     | 否    | The JDBC partition number, default 1.             |
| geaflow.dsl.jdbc.partition.column     | 否    | The JDBC partition column. Default value is 'id'. |
| geaflow.dsl.jdbc.partition.lowerbound     | 否    | The lowerbound of JDBC partition, just used to decide the partition stride, not for filtering the rows in table.                                 |
| geaflow.dsl.jdbc.partition.upperbound     | 否    | The upperbound of JDBC partition, just used to decide the partition stride, not for filtering the rows in table.                            |
表格内容描述: 表格列名包括“参数名”、“是否必须”和“描述”。逐行描述如下：  
1. "geaflow.dsl.jdbc.driver" 是必须参数，表示 JDBC 驱动程序。  
2. "geaflow.dsl.jdbc.url" 是必须参数，表示数据库的 URL。  
3. "geaflow.dsl.jdbc.username" 是必须参数，表示数据库用户名。  
4. "geaflow.dsl.jdbc.password" 是必须参数，表示数据库密码。  
5. "geaflow.dsl.jdbc.table.name" 是必须参数，表示数据表的名称。  
6. "geaflow.dsl.jdbc.partition.num" 不是必须参数，表示 JDBC 分区数量，默认值为 1。  
7. "geaflow.dsl.jdbc.partition.column" 不是必须参数，表示 JDBC 分区列，默认值为 'id'。  
8. "geaflow.dsl.jdbc.partition.lowerbound" 不是必须参数，表示 JDBC 分区的下界，用于决定分区步幅，并不用于过滤表中的行。  
9. "geaflow.dsl.jdbc.partition.upperbound" 不是必须参数，表示 JDBC 分区的上界，同样用于决定分区步幅，而非过滤表中的行。  

总体而言，该表格列出了与 JDBC 相关的参数设置，其中包括多项必需参数和一些可选参数，确保用户在进行数据连接和分区时能有效地配置相关信息。


## 示例

```sql
set geaflow.dsl.jdbc.driver = 'org.h2.Driver';
set geaflow.dsl.jdbc.url = 'jdbc:h2:mem:testdb;DB_CLOSE_DELAY=-1';
set geaflow.dsl.jdbc.username = 'h2_user';
set geaflow.dsl.jdbc.password = 'h2_pwd'; 

CREATE TABLE jdbc_source_table (
  id BIGINT,
  name VARCHAR,
  age INT
) WITH (
	type='jdbc',
    geaflow.dsl.jdbc.table.name = 'source_table',
);

CREATE TABLE jdbc_sink_table (
  id BIGINT,
  name VARCHAR,
  age INT
) WITH (
	type='jdbc',
    geaflow.dsl.jdbc.table.name = 'sink_table'
);

INSERT INTO jdbc_sink_table
SELECT * FROM jdbc_source_table;
```