# Kafka Connector介绍
GeaFlow 支持从 Kafka 中读取数据，并向 Kafka 写入数据。目前支持的 Kafka 版本为 2.4.1。
## 语法

```sql
CREATE TABLE kafka_table (
  id BIGINT,
  name VARCHAR,
  age INT
) WITH (
	type='kafka',
    geaflow.dsl.kafka.servers = 'localhost:9092',
	geaflow.dsl.kafka.topic = 'test-topic'
)
```
## 参数

| 参数名 | 是否必须 | 描述 |
| -------- | -------- | -------- |
| geaflow.dsl.kafka.servers     | 是     | Kafka 的引导服务器（bootstrap）列表     |
| geaflow.dsl.kafka.topic     | 是     | Kafka topic|
| geaflow.dsl.kafka.group.id     | 否     | Kafka组（group id），默认是'default-group-id'.|
表格内容描述: 该表格包含三个列名：参数名、是否必须以及描述。具体数据内容如下：

1. 参数名为 "geaflow.dsl.kafka.servers"，该参数是必填的，描述为 "Kafka 的引导服务器（bootstrap）列表"。
2. 参数名为 "geaflow.dsl.kafka.topic"，该参数也是必填的，描述为 "Kafka topic"。
3. 参数名为 "geaflow.dsl.kafka.group.id"，该参数并非必填，其默认值为 'default-group-id'，描述为 "Kafka组（group id）"。

总体总结：此表格详细列出了与Kafka配置相关的参数，包括它们是否为必填项及其简要说明。主要参数包括引导服务器列表、topic和可选的组ID，其中前两个参数是必填的，而组ID有默认值。


## 示例

```sql
CREATE TABLE kafka_source (
  id BIGINT,
  name VARCHAR,
  age INT
) WITH (
	type='kafka',
    geaflow.dsl.kafka.servers = 'localhost:9092',
	geaflow.dsl.kafka.topic = 'read-topic'
);

CREATE TABLE kafka_sink (
  id BIGINT,
  name VARCHAR,
  age INT
) WITH (
	type='kafka',
    geaflow.dsl.kafka.servers = 'localhost:9092',
	geaflow.dsl.kafka.topic = 'write-topic'
);

INSERT INTO kafka_sink
SELECT * FROM kafka_source;
```