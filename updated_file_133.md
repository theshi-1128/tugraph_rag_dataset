# Hbase Connector介绍
Hbase Connector由社区贡献，目前仅支持Sink。

## 语法示例

```sql
CREATE TABLE hbase_table (
  id BIGINT,
  name VARCHAR,
  age INT
) WITH (
	type='hbase',
    geaflow.dsl.hbase.zookeeper.quorum = '127.0.0.1',
    geaflow.dsl.hbase.tablename = 'GeaFlowBase',
    geaflow.dsl.hbase.rowkey.column = 'id'
);
```
## 参数

| 参数名 | 是否必须 | 描述 |
| -------- | -------- | -------- |
| geaflow.dsl.hbase.zookeeper.quorum     | 是     | HBase zookeeper quorum servers list.     |
| geaflow.dsl.hbase.namespace     | 否     | HBase namespace.     |
| geaflow.dsl.hbase.tablename     | 是     | HBase table name.     |
| geaflow.dsl.hbase.rowkey.column     | 是     | HBase rowkey columns.     |
| geaflow.dsl.hbase.rowkey.separator     | 否     | HBase rowkey join serapator.     |
| geaflow.dsl.hbase.familyname.mapping     | 否     | HBase column family name mapping.     |
| geaflow.dsl.hbase.buffersize     | 否     | HBase writer buffer size.     |
表格内容描述:

该表格包含三个列名：参数名、是否必须、描述。

1. 第一行：参数名为 "geaflow.dsl.hbase.zookeeper.quorum"，该参数是必须的，描述为 HBase 的 zookeeper quorum 服务器列表。
2. 第二行：参数名为 "geaflow.dsl.hbase.namespace"，该参数不是必须的，描述为 HBase 的命名空间。
3. 第三行：参数名为 "geaflow.dsl.hbase.tablename"，该参数是必须的，描述为 HBase 的表名。
4. 第四行：参数名为 "geaflow.dsl.hbase.rowkey.column"，该参数是必须的，描述为 HBase 的 rowkey 列。
5. 第五行：参数名为 "geaflow.dsl.hbase.rowkey.separator"，该参数不是必须的，描述为 HBase rowkey 的连接分隔符。
6. 第六行：参数名为 "geaflow.dsl.hbase.familyname.mapping"，该参数不是必须的，描述为 HBase 列族名称映射。
7. 第七行：参数名为 "geaflow.dsl.hbase.buffersize"，该参数不是必须的，描述为 HBase 写入缓冲区大小。

整体总结：该表格详细列出了与 HBase 配置相关的参数，其中包含 4 个必须参数和 3 个可选参数，涉及 zookeeper 服务器列表、命名空间、表名、rowkey 配置以及读写缓冲设置，明确了每个参数的功能与必要性。

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

CREATE TABLE hbase_table (
  id BIGINT,
  name VARCHAR,
  age INT
) WITH (
	type='hbase',
    geaflow.dsl.hbase.zookeeper.quorum = '127.0.0.1',
    geaflow.dsl.hbase.tablename = 'GeaFlowBase',
    geaflow.dsl.hbase.rowkey.column = 'id'
);

INSERT INTO hbase_table
SELECT * FROM file_source;
```