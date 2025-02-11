# Rust 存储过程

## 1. 介绍
Rust 存储过程目前仅支持v1版本，TuGraph能够支持一切编译成动态库的语言作为插件。Rust语言作为系统编程语言的新起之秀，在安全性上、可靠性以及人体工程学上相较于C++具有较大优势。

我们提供了TuGraph的 Rust binding 库来支持在Rust中调用lgrahp api，同时提供 tugraph-plugin-util 工具库来帮助大家更加简洁地编写Rust插件代码。

Rust binding 链接: https://crates.io/crates/tugraph
tugraph-plugin-util 链接: https://crates.io/crates/tugraph-plugin-util

## 2. 如何使用

Rust存储过程的使用分三步：
* 编译，从rust源码编译出so库。我们准备了一份一站式的插件编写教程，从IDE的插件安装，环境配置，到编译，详细参考`rust-tugraph-plugin-tutorial`。
* 加载，将so库加载到服务端，可以通过REST或RPC接口，这一步和C++库的使用方式类似。
* 运行，和c++ procdure使用方式相同，不在赘述。

## 3.API文档
Rust社区习惯，所有的代码和文档都可以从[`crates.io`](https://crates.io/crates/tugraph )以及[`docs.rs`](https://docs.rs/tugraph/latest/tugraph )找到。
