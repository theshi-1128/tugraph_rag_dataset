# Hive Connector介绍
GeaFlow 支持通过 Hive metastore 服务器读取 Hive 表中的数据。目前，我们支持 Hive 2.3.x系列版本。
## 语法

```sql
CREATE TABLE hive_table (
  id BIGINT,
  name VARCHAR,
  age INT
) WITH (
	type='hive',
    geaflow.dsl.hive.database.name = 'default',
	geaflow.dsl.hive.table.name = 'user',
	geaflow.dsl.hive.metastore.uris = 'thrift://localhost:9083'
)
```
## 参数

| 参数名 | 是否必须 | 描述 |
| -------- | ---- | -------- |
| geaflow.dsl.hive.database.name     | 是 | Hive数据库名字     |
| geaflow.dsl.hive.table.name     | 是 | Hive表名     |
| geaflow.dsl.hive.metastore.uris     | 是 | 连接Hive元数据metastore的uri列表     |
| geaflow.dsl.hive.splits.per.partition     | 否 | 每个Hive分片的逻辑分片数量，默认为1     |
表格内容描述: 

该表格包含三个列名：参数名、是否必须和描述。 

- 第一行数据：参数名为 "geaflow.dsl.hive.database.name"，该参数是必须的，描述为 "Hive数据库名字"。
- 第二行数据：参数名为 "geaflow.dsl.hive.table.name"，该参数是必须的，描述为 "Hive表名"。
- 第三行数据：参数名为 "geaflow.dsl.hive.metastore.uris"，该参数是必须的，描述为 "连接Hive元数据metastore的uri列表"。
- 第四行数据：参数名为 "geaflow.dsl.hive.splits.per.partition"，该参数不是必须的，描述为 "每个Hive分片的逻辑分片数量，默认为1"。

总结：该表格列出了与Hive数据库相关的多个参数，其中前三个参数是必填项，用于配置数据库、表名以及元数据连接信息，最后一个参数为可选项，指定每个分片的逻辑分片数量。

## 示例

```sql
CREATE TABLE hive_table (
  id BIGINT,
  name VARCHAR,
  age INT
) WITH (
	type='hive',
    geaflow.dsl.hive.database.name = 'default',
	geaflow.dsl.hive.table.name = 'user',
	geaflow.dsl.hive.metastore.uris = 'thrift://localhost:9083'
);

CREATE TABLE console (
  id BIGINT,
  name VARCHAR,
  age INT
) WITH (
	type='console'
);

INSERT INTO console
SELECT * FROM hive_table;
```