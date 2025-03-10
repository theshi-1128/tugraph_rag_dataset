# 从源码编译

> 本文档主要描述 TuGraph 从源码进行编译。

## 1.前置条件

推荐在linux系统下搭建TuGraph。同时docker环境是个不错的选择。

## 2.编译介绍

以下是编译TuGraph的步骤：

1. 如果需要web接口运行`deps/build_deps.sh`，不需要web接口则跳过此步骤
2. 根据容器系统信息执行`cmake .. -DOURSYSTEM=centos`或者`cmake .. -DOURSYSTEM=ubuntu`，如果在arm机器编译（如M1芯片的Mac中，需要加上` -DENABLE_BUILD_ON_AARCH64=ON`）
3. `make`
4. `make package` 或者 `cpack --config CPackConfig.cmake`

示例：`tugraph/tugraph-compile-centos7`Docker环境

```bash
$ git clone --recursive https://github.com/TuGraph-family/tugraph-db.git
$ cd tugraph-db
$ deps/build_deps.sh
$ mkdir build && cd build
$ cmake .. -DOURSYSTEM=centos7
$ make
$ make package
```
