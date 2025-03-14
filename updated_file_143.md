# Hudi Connector介绍
GeaFlow Hudi 目前支持从文件中读取数据。
## 语法

```sql
CREATE TABLE IF NOT EXISTS hudi_person (
  id BIGINT,
  name VARCHAR
) WITH (
  type='hudi', 
  geaflow.file.persistent.config.json = '{\'fs.defaultFS\':\'namenode:9000\'}',
  geaflow.dsl.file.path='/path/to/hudi_person'
);
```
## 参数

| 参数名 | 是否必须 | 描述 |
| -------- | -------- | -------- |
| geaflow.dsl.file.path     | 是     | 读取或写入的文件或文件夹的路径     |
| geaflow.file.persistent.config.json     | 否    | JSON格式的DFS配置，会覆盖系统环境配置。      |
表格内容描述: 
该表格包含三列，分别为“参数名”、“是否必须”和“描述”。 

第一行数据是“geaflow.dsl.file.path”，其“是否必须”为“是”，描述为“读取或写入的文件或文件夹的路径”。 
第二行数据是“geaflow.file.persistent.config.json”，其“是否必须”为“否”，描述为“JSON格式的DFS配置，会覆盖系统环境配置”。 

总体来看，该表格列出了两个参数的相关信息，其中“geaflow.dsl.file.path”是必需的参数，而“geaflow.file.persistent.config.json”则是可选的参数。

## 示例

```sql
set geaflow.dsl.window.size = -1;

CREATE TABLE IF NOT EXISTS hudi_person (
  id BIGINT,
  name VARCHAR
) WITH (
   type='hudi', -- hdfs 配置，也可通过HADOOP_HOME环境变量获取
  `geaflow.file.persistent.config.json` = '{\'fs.defaultFS\':\'namenode:9000\'}',
	geaflow.dsl.file.path='/path/to/hudi_person'
);

CREATE TABLE IF NOT EXISTS hudi_sink (
  id BIGINT,
  name VARCHAR
) WITH (
  type='hudi', -- hdfs 配置，也可通过HADOOP_HOME环境变量获取
  `geaflow.file.persistent.config.json` = '{\'fs.defaultFS\':\'namenode:9000\'}',
	geaflow.dsl.file.path='/path/to/hudi_sink'
);

INSERT INTO hudi_sink
SELECT * FROM hudi_person;
```