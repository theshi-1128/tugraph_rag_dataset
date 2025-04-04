# 日志信息

> 此文档主要介绍 TuGraph 的日志功能。

## 1.简介

TuGraph 保留两种类型的日志：服务器日志和审计日志。服务器日志记录人为可读的服务器状态信息，而审核日志维护服务器上执行的每个操作加密后的信息。

## 2.服务器日志

### 2.1.服务器日志配置项

服务器日志的输出位置可以通过`log_dir`配置指定。服务器日志详细程度可通过`verbose`配置项指定。

`log_dir`配置项默认为空。若`log_dir`配置项为空，则所有日志会输出到控制台(daemon模式下若log_dir配置项为空则不会向console输出任何日志)；若手动指定`log_dir`配置项，则日志文件会生成在对应的路径下面。单个日志文件最大大小为256MB。

`verbose`配置项控制日志的详细程度，从粗到细分为`0, 1, 2`三个等级，默认等级为`1`。等级为`2`时，日志记录最详细，服务器将打印`DEBUG`及以上等级的全部日志信息；等级为`1`时，服务器将仅打印`INFO`等级及以上的主要事件的日志；等级为`0`时，服务器将仅打印`ERROR`等级及以上的错误日志。

### 2.2.服务器日志输出宏使用示例

如果开发者在开发过程中希望在代码中添加日志，可以参考如下示例

```
#include "tools/lgraph_log.h" //添加日志依赖


void LogExample() {
    // 数据库启动阶段已经对日志模块进行了初始化，开发者只需直接调用宏即可
    // 日志等级分为DEBUG, INFO, WARNING, ERROR, FATAL五个等级
    LOG_DEBUG() << "This is a debug level log message.";
    LOG_INFO() << "This is a info level log message.";
    LOG_WARN() << "This is a warning level log message.";
    LOG_ERROR() << "This is a error level log message.";
    LOG_FATAL() << "This is a fatal level log message.";
}
```
更多用法可以参考test/test_lgraph_log.cpp中的日志宏的使用方法

### 2.3.存储过程日志

用户在存储过程的编写过程中可以使用日志功能将所需的调试信息输出到日志中进行查看，辅助开发。调试信息会输出到与服务器日志相同的日志文件中(如未指定`log_dir`则同样输出至console)

#### 2.3.1.cpp存储过程
请使用2.2中提供的log宏输出调试信息，避免使用cout或者printf等输出方式。具体使用方式可参考如下示例代码（详见`procedures/demo/log_demo.cpp`）

```
#include <stdlib.h>
#include "lgraph/lgraph.h"
#include "tools/lgraph_log.h"  // add log dependency
using namespace lgraph_api;

void LogExample() {
    LOG_DEBUG() << "This is a debug level log message.";
    LOG_INFO() << "This is a info level log message.";
    LOG_WARN() << "This is a warning level log message.";
    LOG_ERROR() << "This is a error level log message.";
}

extern "C" bool Process(GraphDB& db, const std::string& request, std::string& response) {
    response = "TuGraph log demo";
    LogExample();
    return true;
}
```
将以上示例代码作为存储过程插入数据库并运行后，可以在日志文件中看到相应的日志条目。

#### 2.3.1.python存储过程
请使用python自带的print输出调试信息，调试信息会在存储过程运行结束后合并为一条WARN等级的日志条目输出至日志文件中。

## 3.审计日志

审核日志记录每个请求和响应，以及发送请求的用户以及收到请求的时间。审核日志只能是打开或关闭状态。可以使用 TuGraph 可视化工具和 REST API 查询结果。

开启审计日志需要在配置文件中将`enable_audit_log`参数设置为`true`。
