# Procedure API

> 此文档主要讲解 TuGraph 的存储过程使用说明（这个文档描述了新的 Procedure 编程范式以及 POG）

## 1.简介

当用户需要表达的查询/更新逻辑较为复杂（例如 Cypher 无法描述，或是对性能要求较高）时，相比调用多个请求并在客户端完成整个处理流程的方式，TuGraph 提供的存储过程是更简洁和高效的选择。

与传统数据库类似，TuGraph 的存储过程运行在服务器端，用户通过将处理逻辑（即多个操作）封装到一个过程单次调用，并且可以在实现时通过并行处理的方式（例如使用相关的 C++ OLAP 接口以及基于其实现的内置算法）进一步提升性能。

存储过程中有一类特殊的API来进行数据的并行操作，我们叫 Traversal API。

## 2.存储过程的版本支持

目前TuGraph支持两个版本的存储过程，适用于不同的场景，v3.5版本只支持v1，可通过REST或RPC接口直接调用；从v3.5版本开始支持v2，能够在图查询语言（比如Cypher）中嵌入调用，我们称之为POG（Procedure On Graph query language，APOC）。

|                        | Procedure v1                       | Procedure v2               |
| ---------------------- | ---------------------------------- | -------------------------- |
| 适用场景                 | 极致性能，或者复杂的多事务管理情形       | 一般情况，与Cypher高度联动 |
| 事务                    | 函数内部创建，可自由控制多事务          | 外部传入函数，单一事务     |
| 签名（参数定义）          | 无                                 | 有                    |
| 输入输出参数类型          | 不需要指定                           | 需要指定参数类型        |
| Cypher Standalone Call | 支持                                | 支持                  |
| Cypher Embeded Call    | 不支持                              | 支持                  |
| 语言                    | C++/Python/Rust                    | C++                  |
| 调用模式                 | 直接传字符串，一般为JSON               | 通过Cypher语句中的变量  |
表格内容描述：
表格中包含两列，分别为“Procedure v1”和“Procedure v2”。 

- 在“适用场景”这一行，Procedure v1适用于极致性能或复杂的多事务管理情形，而Procedure v2则适用于一般情况，与Cypher的联动性较强。
- 在“事务”一行，Procedure v1中事务在函数内部创建，允许自由控制多事务；相对而言，Procedure v2的事务则是由外部传入函数，且为单一事务。
- “签名（参数定义）”一行显示，Procedure v1没有必要的参数定义，而Procedure v2则需要明确的参数定义。
- 在“输入输出参数类型”一行，Procedure v1不需要指定参数类型，而Procedure v2则需要指定。
- 关于“Cypher Standalone Call”，两种Procedure均支持。
- 在“Cypher Embeded Call”一行，Procedure v1不支持嵌入式调用，而Procedure v2则支持。
- 在“语言”一行，Procedure v1可以使用C++、Python或Rust语言，而Procedure v2则只支持C++。
- 最后在“调用模式”中，Procedure v1直接传入字符串，通常为JSON格式；而Procedure v2则通过Cypher语句中的变量进行调用。

总结来说，该表格对比了Procedure v1和Procedure v2在适用场景、事务处理、参数定义、参数类型、Cypher调用方式、支持语言及调用模式等方面的差异，表明Procedure v1更适合性能优先的复杂事务场景，而Procedure v2更适合与Cypher交互的常规操作。

在TuGraph中，存储过程v1和v2单独管理，支持增删查，但仍不建议重名。

## 3.存储过程语言支持

在 TuGraph 中，用户可以动态的加载，更新和删除存储过程。TuGraph 支持 C++ 语言、 Python 语言和 Rust 语言编写存储过程。在性能上 C++ 语言支持的最完整，性能最优。

注意存储过程是在服务端编译执行的逻辑，和客户端的语言支持无关。

## 4.Procedure v1接口

## 4.1.编写存储过程

### 4.1.1.编写C++存储过程

用户可以通过使用 Procedure API 或者 Traversal API 来编写 C 存储过程。一个简单的 C 存储过程举例如下：

```
#include <iostream>
#include "lgraph.h"
using namespace lgraph_api;

extern "C" LGAPI bool Process(GraphDB& db, const std::string& request, std::string& response) {
	auto txn = db.CreateReadTxn();
	size_t n = 0;
	for (auto vit = txn.GetVertexIterator(); vit.IsValid(); vit.Next()) {
        if (vit.GetLabel() == "student") {
            auto age = vit.GetField("age");
            if (!age.is_null() && age.integer() == 10) n++; ## 统计所有年龄为10的学生数量
        }
	}
    output = std::to_string(n);
    return true;
}
```

从代码中我们可以看到，存储过程的入口函数是`Process`函数，它的参数有三个，分别为：

- `db`: 数据库实例
- `request`: 输入请求数据，可以是二进制字节数组，或者 JSON 串等其它任意格式。
- `response`: 输出数据，可以是字符串，也可以直接返回二进制数据。

`Process`函数的返回值是一个布尔值。当它返回`true`的时候，表示该请求顺利完成，反之表示这个存储过程在执行过程中发现了错误，此时用户可以通过`response`来返回错误信息以方便调试。

C++存储过程编写完毕后需要编译成动态链接库。TuGraph 提供了`compile.sh`脚本来帮助用户自动编译存储过程。`compile.sh`脚本只有一个参数，是该存储过程的名称，在上面的例子中就是`age_10`。编译调用命令行如下：

```bash
g++ -fno-gnu-unique -fPIC -g --std=c++14 -I/usr/local/include/lgraph -rdynamic -O3 -fopenmp -o age_10.so age_10.cpp /usr/local/lib64/liblgraph.so -shared
```

如果编译顺利，会生成 age_10.so，然后用户就可以将它加载到服务器中了。

### 4.1.2.编写Python存储过程

与 C++类似，Python 存储过程也可以调用 core API，一个简单的例子如下：

```python
def Process(db, input):
    txn = db.CreateReadTxn()
    it = txn.GetVertexIterator()
    n = 0
    while it.IsValid():
        if it.GetLabel() == 'student' and it['age'] and it['age'] == 10:
            n = n + 1
        it.Next()
    return (True, str(nv))
```

Python 存储过程返回的是一个 tuple，其中第一个元素是一个布尔值，表示该存储过程是否成功执行；第二个元素是一个`str`，里面是需要返回的结果。

Python 存储过程不需要编译，可以直接加载。

## 4.2.如何使用存储过程
### 4.2.1.加载存储过程

用户可以通过 REST API 和 RPC 来加载存储过程。以 REST API 为例，加载`age_10.so`的 C++代码如下：

```python
import requests
import json
import base64

data = {'name':'age_10'}
f = open('./age_10.so','rb')
content = f.read()
data['code_base64'] = base64.b64encode(content).decode()
data['description'] = 'Custom Page Rank Procedure'
data['read_only'] = true
data['code_type'] = 'so'
js = json.dumps(data)
r = requests.post(url='http://127.0.0.1:7071/db/school/cpp_plugin', data=js,
            headers={'Content-Type':'application/json'})
print(r.status_code)    ## 正常时返回200
```

需要注意的是，这时的`data['code']`是一个经过 base64 处理的字符串，`age_10.so`中的二进制代码是无法通过 JSON 直接传输的。此外，存储过程的加载和删除都只能由具有管理员权限的用户来操作。

存储过程加载之后会被保存在数据库中，在服务器重启后也会被自动加载。此外，如果需要对存储过程进行更新，调用的 REST API 也是同样的。建议用户在更新存储过程时更新相应描述，以便区分不同版本的存储过程。


### 4.2.2.列出已加载的存储过程

在服务器运行过程中，用户可以随时获取存储过程列表。其调用如下：

```python
>>> r = requests.get('http://127.0.0.1:7071/db/school/cpp_plugin')
>>> r.status_code
200
>>> r.text
'{"plugins":[{"description":"Custom Page Rank Procedure", "name":"age_10", "read_only":true}]}'
```

### 4.2.3.获取存储过程详情

在服务器运行过程中，用户可以随时获取单个存储过程的详情，包括代码。其调用如下：

```python
>>> r = requests.get('http://127.0.0.1:7071/db/school/cpp_plugin/age_10')
>>> r.status_code
200
>>> r.text
'{"description":"Custom Page Rank Procedure", "name":"age_10", "read_only":true, "code_base64":<CODE>, "code_type":"so"}'
```

### 4.2.4.调用存储过程

调用存储过程的代码示例如下：

```
>>> r = requests.post(url='http://127.0.0.1:7071/db/school/cpp_plugin/age_10', data='',
                headers={'Content-Type':'application/json'})
>>> r.status_code
200
>>> r.text
9
```

### 4.2.5.删除存储过程

删除存储过程只需要如下调用：

```python
>>> r = requests.delete(url='http://127.0.0.1:7071/db/school/cpp_plugin/age_10')
>>> r.status_code
200
```

与加载存储过程类似，只有管理员用户才能删除存储过程。

### 4.2.6.更新存储过程

更新存储过程需要执行如下两个步骤：

1.  删除已存在的存储过程
2.  安装新的存储过程

TuGraph 较为谨慎地管理存储过程操作的并发性，更新存储过程不会影响现有存储过程的运行。


## 5.Procedure v2接口

下面的说明以 REST API 为例，介绍存储过程v2的调用。

### 5.1.编写存储过程


用户可以通过使用 lgraph API 来编写 C++ 存储过程。一个简单的 C++ 存储过程举例如下：

```c++
// peek_some_node_salt.cpp
#include <cstdlib>
#include "lgraph/lgraph.h"
#include "lgraph/lgraph_types.h"
#include "lgraph/lgraph_result.h"

#include "tools/json.hpp"

using json = nlohmann::json;
using namespace lgraph_api;

extern "C" LGAPI bool GetSignature(SigSpec &sig_spec) {
    sig_spec.input_list = {
        {.name = "limit", .index = 0, .type = LGraphType::INTEGER},
    };
    sig_spec.result_list = {
        {.name = "node", .index = 0, .type = LGraphType::NODE},
        {.name = "salt", .index = 1, .type = LGraphType::FLOAT}
    };
    return true;
}

extern "C" LGAPI bool ProcessInTxn(Transaction &txn,
                                   const std::string &request,
                                   Result &response) {
    int64_t limit;
    try {
        json input = json::parse(request);
        limit = input["limit"].get<int64_t>();
    } catch (std::exception &e) {
        response.ResetHeader({
            {"errMsg", LGraphType::STRING}
        });
        response.MutableRecord()->Insert(
            "errMsg",
            FieldData::String(std::string("error parsing json: ") + e.what()));
        return false;
    }

    response.ResetHeader({
        {"node", LGraphType::NODE},
        {"salt", LGraphType::FLOAT}
    });
    for (size_t i = 0; i < limit; i++) {
        auto r = response.MutableRecord();
        auto vit = txn.GetVertexIterator(i);
        r->Insert("node", vit);
        r->Insert("salt", FieldData::Float(20.23*float(i)));
    }
    return true;
}
```

从代码中我们可以看到：
- 存储过程定义了一个获取签名的方法`GetSignature`。该方法返回了存储过程的签名，其中包含输入参数名称及其类型，返回参数及其类型。这使得Cypher查询语句在调用存储过程能够利用签名信息校验输入数据以及返回数据是否合理。
- 入口函数是`ProcessInTxn`函数，它的参数有三个，分别为：

- `txn`: 存储过程所处的事务，通常来说即调用该存储过程的Cypher语句所处事务。
- `request`: 输入数据，其内容为`GetSignature`中定义的输入参数类型及其Cypher查询语句中传入的值经过json序列化后的字符串。e.g. `{num_iteration: 10}`
- `response`: 输出数据，为保证在Cypher语言中能够兼容，用户可以通过往`lgraph_api::Result` 写入存储过程处理后的数据，最后用`lgraph_api::Result::Dump`来序列化成json格式的数据。

`ProcessInTxn`函数的返回值是一个布尔值。当它返回`true`的时候，表示该请求顺利完成，反之表示这个存储过程在执行过程中发现了错误。

C++存储过程编写完毕后需要编译成动态链接库。TuGraph 提供了`compile.sh`脚本来帮助用户自动编译存储过程。`compile.sh`脚本只有一个参数，是该存储过程的名称，在上面的例子中就是`custom_pagerank`。编译调用命令行如下：

```bash
g++ -fno-gnu-unique -fPIC -g --std=c++14 -I/usr/local/include/lgraph -rdynamic -O3 -fopenmp -o custom_pagerank.so custom_pagerank.cpp /usr/local/lib64/liblgraph.so -shared
```

如果编译顺利，会生成 custom_pagerank.so，然后用户就可以将它加载到服务器中了。


### 5.2.加载存储过程

用户可以通过 REST API 和 RPC 来加载存储过程。以 REST API 为例，加载`custom_pagerank.so`的 C++代码如下：

```python
import requests
import json
import base64

data = {'name':'custom_pagerank'}
f = open('./custom_pagerank.so','rb')
content = f.read()
data['code_base64'] = base64.b64encode(content).decode()
data['description'] = 'Custom Page Rank Procedure'
data['read_only'] = true
data['code_type'] = 'so'
js = json.dumps(data)
r = requests.post(url='http://127.0.0.1:7071/db/school/cpp_plugin', data=js,
            headers={'Content-Type':'application/json'})
print(r.status_code)    ## 正常时返回200
```

需要注意的是，这时的`data['code']`是一个经过 base64 处理的字符串，`custom_pagerank.so`中的二进制代码是无法通过 JSON 直接传输的。此外，存储过程的加载和删除都只能由具有管理员权限的用户来操作。

存储过程加载之后会被保存在数据库中，在服务器重启后也会被自动加载。此外，如果需要对存储过程进行更新，调用的 REST API 也是同样的。建议用户在更新存储过程时更新相应描述，以便区分不同版本的存储过程。


#### 5.2.1.列出已加载的存储过程

在服务器运行过程中，用户可以随时获取存储过程列表。其调用如下：

```python
>>> r = requests.get('http://127.0.0.1:7071/db/school/cpp_plugin')
>>> r.status_code
200
>>> r.text
'{"plugins":[{"description":"Custom Page Rank Procedure", "name":"custom_pagerank", "read_only":true}]}'
```

#### 5.2.2.获取存储过程详情

在服务器运行过程中，用户可以随时获取单个存储过程的详情，包括代码。其调用如下：

```python
>>> r = requests.get('http://127.0.0.1:7071/db/school/cpp_plugin/custom_pagerank')
>>> r.status_code
200
>>> r.text
'{"description":"Custom Page Rank Procedure", "name":"custom_pagerank", "read_only":true, "code_base64":<CODE>, "code_type":"so"}'
```

#### 5.2.3.调用存储过程

调用存储过程的代码示例如下：

```Cypher
CALL plugin.cpp.custom_pagerank(10)
YIELD node, pr WITH node, pr
MATCH(node)-[r]->(n) RETURN node, r, n, pr
```

#### 5.2.4.删除存储过程

删除存储过程只需要如下调用：

```python
>>> r = requests.delete(url='http://127.0.0.1:7071/db/school/cpp_plugin/custom_pagerank')
>>> r.status_code
200
```

与加载存储过程类似，只有管理员用户才能删除存储过程。

#### 5.2.5.更新存储过程

更新存储过程需要执行如下两个步骤：

1.  删除已存在的存储过程
2.  安装新的存储过程

TuGraph 较为谨慎地管理存储过程操作的并发性，更新存储过程不会影响现有存储过程的运行。

