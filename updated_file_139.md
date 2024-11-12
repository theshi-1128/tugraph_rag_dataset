# Python客户端

> 此文档主要是TuGraph Python SDK的使用说明, 注意TuGraph Python SDK将来不再更新维护。

## 1. 概述
Python的TuGraph Client有两种，一种是RESTful的Client，使用HTTP方法向server发送请求，另一种是RPC的Client，使用RPC方法调用server远程服务，两者各有优劣。
RESTful client的使用方式简单，在项目的src/client/python/TuGraphClient目录下可以找到Client的源码文件，直接安装到用户环境中即可使用，但是支持的功能较少，
性能也不高。RPC Client既支持单机的server，也支持高可用集群和负载均衡，接口较多，功能强大。但是使用方式较为复杂，需要用户自己编译TuGraph项目得到liblgraph_client_python.so，
或者使用runtime镜像时直接在/usr/local/lib64目录下找到该依赖库，将其引入python项目即可正常使用。接下来将详细介绍这两种Client的使用方式。

## 2. RESTful Client

### 2.1.安装Client

TuGraph的Python RESTful client使用setuptools工具进行打包和分发，用户可以将client直接安装到本地环境中，在使用时即可直接引入。
```shell
cd src/client/python/TuGraphClient
python3 setup.py build
python3 setup.py install
```
注：使用setuptools工具安装python client时会安装httpx等依赖，需要在通外网的环境下执行。

### 2.2.调用Cypher

```python
from TuGraphClient import TuGraphClient, AsyncTuGraphClient

client = TuGraphClient("127.0.0.1:7071" , "admin", "73@TuGraph")
cypher = "match (n) return properties(n) limit 1"
res = client.call_cypher(cypher)
print(res)

aclient = AsyncTuGraphClient("127.0.0.1:7071" , "admin", "73@TuGraph")
cypher = "match (n) return properties(n) limit 1"
res = await aclient.call_cypher(cypher)
print(res)
```

### 2.3.调用存储过程

```python
from TuGraphClient import TuGraphClient, AsyncTuGraphClient

client = TuGraphClient("127.0.0.1:7071" , "admin", "73@TuGraph")
plugin_type = "cpp"
plugin_name = "khop"
plugin_input = "{\"root\": 10, \"hop\": 3}"
res = client.call_plugin(plugin_type, plguin_name, plugin_input)
print(res)

aclient = AsyncTuGraphClient("127.0.0.1:7071" , "admin", "73@TuGraph")
res = await aclient.call_plugin(plugin_type, plguin_name, plugin_input)
print(res)
```

## 3.RPC Client

Python的TuGraph Rpc Client是使用pybind11包装的CPP Client SDK，下表列出了Python和CPP Client SDK的对应关系。

| Python Client SDK                                                                                                                                                                                                     | CPP Client SDK                                                                                                                                                                                                                                                                              |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| client(self: liblgraph_client_python.client, url: str, user: str, password: str)                                                                                                                                      | RpcClient(const std::string& url, const std::string& user, const std::string& password)                                                                                                                                                                                                     |
| client(self: liblgraph_client_python.client, urls: list, user: str, password: str)                                                                                                                                    | RpcClient(std::vector<std::string>& urls, std::string user, std::string password)                                                                                                                                                                                                           |
| callCypher(self: liblgraph_client_python.client, cypher: str, graph: str, json_format: bool, timeout: float, url: str) -> (bool, str)                                                                                 | bool CallCypher(std::string& result, const std::string& cypher, const std::string& graph, bool json_format, double timeout, const std::string& url)                                                                                                                                         |
| callCypherToLeader(self: liblgraph_client_python.client, cypher: str, graph: str, json_format: bool, timeout: float) -> (bool, str)                                                                                   | bool CallCypherToLeader(std::string& result, const std::string& cypher, const std::string& graph, bool json_format, double timeout)                                                                                                                                                         |
| callGql(self: liblgraph_client_python.client, gql: str, graph: str, json_format: bool, timeout: float, url: str) -> (bool, str)                                                                                       | bool CallGql(std::string& result, const std::string& gql, const std::string& graph, bool json_format, double timeout, const std::string& url)                                                                                                                                               |
| callGqlToLeader(self: liblgraph_client_python.client, gql: str, graph: str, json_format: bool, timeout: float) -> (bool, str)                                                                                         | bool CallGqlToLeader(std::string& result, const std::string& gql, const std::string& graph = "default", bool json_format = true, double timeout = 0)                                                                                                                                        |
| callProcedure(self: liblgraph_client_python.client, procedure_type: str, procedure_name: str, param: str, procedure_time_out: float, in_process: bool, graph: str, json_format: bool, url: str) -> (bool, str)        | bool CallProcedure(std::string& result, const std::string& procedure_type, const std::string& procedure_name, const std::string& param, double procedure_time_out, bool in_process, const std::string& graph, bool json_format, const std::string& url)                                     |
| callProcedureToLeader(self: liblgraph_client_python.client, procedure_type: str, procedure_name: str, param: str, procedure_time_out: float, in_process: bool, graph: str, json_format: bool) -> (bool, str)          | CallProcedureToLeader(std::string& result, const std::string& procedure_type, const std::string& procedure_name, const std::string& param, double procedure_time_out, bool in_process, const std::string& graph, bool json_format)                                                          |
| loadProcedure(self: liblgraph_client_python.client, source_file: str, procedure_type: str, procedure_name: str, code_type: str, procedure_description: str, read_only: bool, version: str, graph: str) -> (bool, str) | bool LoadProcedure(std::string& result, const std::string& source_file, const std::string& procedure_type, const std::string& procedure_name, const std::string& code_type, const std::string& procedure_description, bool read_only, const std::string& version, const std::string& graph) |
| listProcedures(self: liblgraph_client_python.client, procedure_type: str, version: str, graph: str, url: str) -> (bool, str)                                                                                          | bool ListProcedures(std::string& result, const std::string& procedure_type, const std::string& version, const std::string& graph, const std::string& url)                                                                                                                                   |
| deleteProcedure(self: liblgraph_client_python.client, procedure_type: str, procedure_name: str, graph: str) -> (bool, str)                                                                                            | bool DeleteProcedure(std::string& result, const std::string& procedure_type, const std::string& procedure_name, const std::string& graph)                                                                                                                                                   |
| importSchemaFromContent(self: liblgraph_client_python.client, schema: str, graph: str, json_format: bool, timeout: float) -> (bool, str)                                                                              | bool ImportSchemaFromContent(std::string& result, const std::string& schema, const std::string& graph, bool json_format, double timeout)                                                                                                                                                    |
| importDataFromContent(self: liblgraph_client_python.client, desc: str, data: str, delimiter: str, continue_on_error: bool, thread_nums: int, graph: str, json_format: bool, timeout: float) -> (bool, str)            | ImportDataFromContent(std::string& result, const std::string& desc, const std::string& data, const std::string& delimiter, bool continue_on_error, int thread_nums, const std::string& graph, bool json_format, double timeout)                                                             |
| importSchemaFromFile(self: liblgraph_client_python.client, schema_file: str, graph: str, json_format: bool, timeout: float) -> (bool, str)                                                                            | ImportSchemaFromFile(std::string& result, const std::string& schema_file, const std::string& graph, bool json_format, double timeout)                                                                                                                                                       |
| importDataFromFile(self: liblgraph_client_python.client, conf_file: str, delimiter: str, continue_on_error: bool, thread_nums: int, skip_packages: int, graph: str, json_format: bool, timeout: float) -> (bool, str) | ImportDataFromFile(std::string& result, const std::string& conf_file, const std::string& delimiter, bool continue_on_error, int thread_nums, int skip_packages, const std::string& graph, bool json_format, double timeout)                                                                 |
表格内容描述: 

该表格包含两个列，分别是“Python Client SDK”和“CPP Client SDK”。表格详细列出了与数据库交互相关的不同方法，并在两种编程语言（Python 和 C++）中进行了对比。每一方法包含方法名称、输入参数及返回值的信息。

逐行描述如下：
1. Python SDK的`client`方法接受一个URL、用户名和密码，并返回一个客户端实例；CPP SDK的`RpcClient`构造函数具有相似的功能。
2. Python SDK的`client`方法也支持多个URL输入；对应的CPP SDK则使用`std::vector`接收多个URL。
3. Python SDK的`callCypher`方法用于执行Cypher查询，具有多个参数并返回查询结果；CPP SDK的对应方法`CallCypher`使用不同类型的参数传递。
4. Python SDK的`callCypherToLeader`方法与 `callCypher`类似，但专门用于与数据领导者通信；CPP SDK的`CallCypherToLeader`也具有类似功能。
5. Python SDK的`callGql`方法和CPP SDK的`CallGql`方法均用于执行GQL查询，输入参数设置类似，返回结果形式一致。
6. Python SDK的`callGqlToLeader`是针对GQL查询的领导者调用版本；CPP SDK也有对应的`CallGqlToLeader`。
7. Python SDK的`callProcedure`用于调用特定过程，接受众多参数；CPP SDK的`CallProcedure`方法功能相似。
8. Python SDK的`callProcedureToLeader`与上一个类似，但更侧重于领导者调用；CPP SDK中有相应的`CallProcedureToLeader`方法。
9. Python SDK的`loadProcedure`用于加载程序，接受详细参数；CPP SDK中的`LoadProcedure`执行相同操作。
10. Python SDK的`listProcedures`列出程序；CPP SDK的`ListProcedures`同样功能。
11. Python SDK的`deleteProcedure`方法用于删除程序；CPP SDK对应的`DeleteProcedure`也有同样功能。
12. Python SDK的`importSchemaFromContent`方法用于从内容导入模式；CPP SDK使用`ImportSchemaFromContent`实现该功能。
13. Python SDK的`importDataFromContent`方法导入数据，而CPP SDK对应了`ImportDataFromContent`。
14. Python SDK的`importSchemaFromFile`方法从文件中导入模式；CPP SDK使用`ImportSchemaFromFile`处理类似操作。
15. Python SDK的`importDataFromFile`处理从文件导入数据，CPP SDK也是通过`ImportDataFromFile`实现同样功能。

整体来说，该表格展示了用于数据库操作的Python和C++ SDK的许多关系和相似点，提供了相应的方法、输入参数及返回值的信息。这为开发者在选择合适的编程语言时提供了清晰的对比和参考。

Python RPC Client的使用方式较为复杂，用户可以在本地环境中编译TuGraph得到liblgraph_client_python.so，也可以使用官方提供的runtime镜像，
在镜像中的/usr/local/lib64目录下可以直接找到该依赖库，引入用户项目即可使用。

```python
import liblgraph_client_python
```

### 3.1.实例化client对象

#### 3.1.1.实例化单节点client对象
当以单节点模式启动server时，client按照如下格式进行实例化
```python
client = liblgraph_client_python.client("127.0.0.1:19099", "admin", "73@TuGraph")
```
```
client(self: liblgraph_client_python.client, url: str, user: str, password: str)
```

#### 3.1.2.实例化HA集群直连连接client对象
当服务器上部署的HA集群可以使用ha_conf中配置的网址直接连接时，client按照如下格式进行实例化。
```python
client = liblgraph_client_python.client("127.0.0.1:19099", "admin", "73@TuGraph")
```
```
client(self: liblgraph_client_python.client, url: str, user: str, password: str)
```
用户只需要传入HA集群中的任意一个节点的url即可，client会根据server端返回的查询信息自动维护连接池，在HA集群横向扩容时
也不需要手动重启client。

#### 3.1.3.实例化HA集群间接连接client对象
当服务器上部署的HA集群不能使用ha_conf中配置的网址直接连接而必须使用间接网址（如阿里云公网网址）连接时，
client按照如下格式进行实例化
```python
client = liblgraph_client_python.client(["189.33.97.23:9091","189.33.97.24:9091", "189.33.97.25:9091"], "admin", "73@TuGraph")
```
```
client(self: liblgraph_client_python.client, urls: list, user: str, password: str)
```
因为用户连接的网址和server启动时配置的信息不同，不能通过向集群发请求的方式自动更新client连接池，所以需要在启动
client时手动传入所有集群中节点的网址，并在集群节点变更时手动重启client。

### 3.2.调用cypher
```python
ret, res = client.callCypher("CALL db.edgeLabels()", "default", 10)
```
```
callCypher(self: liblgraph_client_python.client, cypher: str, graph: str, json_format: bool, timeout: float, url: str) -> (bool, str)
```
本接口支持在单机模式和HA模式下使用。其中，在HA模式下的client中，通过指定url参数可以定向向某个server发送读请求。

### 3.3.向leader发送cypher请求
```python
ret, res = client.callCypherToLeader("CALL db.edgeLabels()", "default", 10)
```
```
callCypherToLeader(self: liblgraph_client_python.client, cypher: str, graph: str, json_format: bool, timeout: float) -> (bool, str)
```
本接口只支持在HA模式下使用，在HA模式下的client中，为防止向未同步数据的follower发送请求，
用户可以直接向leader发送请求，leader由集群选出。

### 3.4.调用GQL
```python
ret, res = client.callGql("CALL db.edgeLabels()", "default", 10)
```
```
callGql(self: liblgraph_client_python.client, gql: str, graph: str, json_format: bool, timeout: float, url: str) -> (bool, str)
```
本接口支持在单机模式和HA模式下使用。其中，在HA模式下的client中，通过指定url参数可以定向向某个server发送读请求。

### 3.5.向leader发送GQL请求
```python
ret, res = client.callGqlToLeader("CALL db.edgeLabels()", "default", 10)
```
```
callGqlToLeader(self: liblgraph_client_python.client, gql: str, graph: str, json_format: bool, timeout: float) -> (bool, str)
```
本接口只支持在HA模式下使用，在HA模式下的client中，为防止向未同步数据的follower发送请求，
用户可以直接向leader发送请求，leader由集群选出。

### 3.6.调用存储过程
```python
ret, res = client.callProcedure("CPP", "test_plugin1", "bcefg", 1000, False, "default")
```
```
callProcedure(self: liblgraph_client_python.client, procedure_type: str, procedure_name: str, param: str, procedure_time_out: float, in_process: bool, graph: str, json_format: bool, url: str) -> (bool, str)
```
本接口支持在单机模式和HA模式下使用，默认以字符串格式直接返回存储过程的执行结果，指定jsonFormat为true可以返回json格式的执行结果。
其中，在HA模式下的client中，通过指定url参数可以定向向某个server发送读请求。

### 3.7.向leader调用存储过程
```python
ret, res = client.callProcedureToLeader("CPP", "khop", kHopParamGen(), 1000, false, "default")
```
```
callProcedureToLeader(self: liblgraph_client_python.client, procedure_type: str, procedure_name: str, param: str, procedure_time_out: float, in_process: bool, graph: str, json_format: bool) -> (bool, str)
```
本接口支持在HA模式下使用，默认以字符串格式直接返回存储过程的执行结果，指定jsonFormat为true可以返回json格式的执行结果。

### 3.8.加载存储过程
```python
ret, res = client.loadProcedure("./test/procedure/khop.so", "CPP", "khop", "SO", "test loadprocedure", true, "v1", "default");
```
```
loadProcedure(self: liblgraph_client_python.client, source_file: str, procedure_type: str, procedure_name: str, code_type: str, procedure_description: str, read_only: bool, version: str, graph: str) -> (bool, str)
```
本接口支持在单机模式和HA模式下使用。其中，由于加载存储过程是写请求，HA模式下的client只能向leader发送加载存储过程请求。

### 3.9.列举存储过程
```python
ret, res = client.listProcedures("CPP", "any", "default")
```
```
listProcedures(self: liblgraph_client_python.client, procedure_type: str, version: str, graph: str, url: str) -> (bool, str)
```
本接口支持在单机模式和HA模式下使用。其中，在HA模式下的client中，通过指定url参数可以定向向某个server发送读请求。

### 3.10.删除存储过程
```python
ret, res = client.deleteProcedure("CPP", "sortstr", "default")
```
```
deleteProcedure(self: liblgraph_client_python.client, procedure_type: str, procedure_name: str, graph: str) -> (bool, str)
```
本接口支持在单机模式和HA模式下使用。其中，由于删除存储过程是写请求，HA模式下的client只能向leader发送删除存储过程请求。

### 3.11.从字节流中导入schema
```python
ret, res = client.importSchemaFromContent(schema, "default", 1000)
```
```
importSchemaFromContent(self: liblgraph_client_python.client, schema: str, graph: str, json_format: bool, timeout: float) -> (bool, str)
```
本接口支持在单机模式和HA模式下使用。其中，由于导入schema是写请求，HA模式下的client只能向leader发送导入schema请求。

### 3.12.从字节流中导入点边数据
```python
ret, res = client.importDataFromContent(personDesc, person, ",", true, 16, "default", 1000)
```
```
importDataFromContent(self: liblgraph_client_python.client, desc: str, data: str, delimiter: str, continue_on_error: bool, thread_nums: int, graph: str, json_format: bool, timeout: float) -> (bool, str)
```
本接口支持在单机模式和HA模式下使用。其中，由于导入点边数据是写请求，HA模式下的client只能向leader发送导入点边数据请求。

### 3.13.从文件中导入schema
```python
ret, res = client.importSchemaFromFile("./test/data/yago.conf", "default", 1000)
```
```
importSchemaFromFile(self: liblgraph_client_python.client, schema_file: str, graph: str, json_format: bool, timeout: float) -> (bool, str)
```
本接口支持在单机模式和HA模式下使用。其中，由于导入schema是写请求，HA模式下的client只能向leader发送导入schema请求。

### 3.14.从文件中导入点边数据
```python
ret, res = client.importDataFromFile("./test/data/yago.conf", ",", true, 16, 0, "default", 1000000000)
```
```
importDataFromFile(self: liblgraph_client_python.client, conf_file: str, delimiter: str, continue_on_error: bool, thread_nums: int, skip_packages: int, graph: str, json_format: bool, timeout: float) -> (bool, str)
```
本接口支持在单机模式和HA模式下使用。其中，由于导入点边数据是写请求，HA模式下的client只能向leader发送导入点边数据请求。
