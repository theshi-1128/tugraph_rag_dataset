# Console Connector介绍

## 语法

```sql
CREATE TABLE console_table (
  id BIGINT,
  name VARCHAR,
  age INT
) WITH (
	type='console',
    geaflow.dsl.console.skip = true
)
```
## 参数

| 参数名 | 是否必须 | 描述                     |
| -------- | -------- |------------------------|
| geaflow.dsl.console.skip     | 否     | 是否跳过日志，即无任何输出，默认为false |
表格内容描述: 本表格包含三列，分别为“参数名”、“是否必须”和“描述”。 

第一行数据描述为：参数名为“geaflow.dsl.console.skip”，其“是否必须”的值为“否”，表示该参数不是必须的。描述部分指出该参数用于控制是否跳过日志输出，默认值为false，即默认情况下会输出日志。

整体而言，此表格提供了关于“geaflow.dsl.console.skip”参数的基本信息，说明该参数可选，且其默认行为为记录日志。

## 示例

```sql
CREATE TABLE file_source (
  id BIGINT,
  name VARCHAR,
  age INT
) WITH (
	type='file',
    geaflow.dsl.file.path = '/path/to/file'
);

CREATE TABLE console_sink (
  id BIGINT,
  name VARCHAR,
  age INT
) WITH (
	type='console'
);

INSERT INTO console_sink
SELECT * FROM file_source;
```