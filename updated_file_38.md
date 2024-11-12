# 本地包部署

> 此文档主要介绍 TuGraph 的本地包部署。

## 1. 环境准备

TuGraph本地包部署需要对应的环境，快速验证可以使用精简安装包，几乎不需要任何第三方库。

如果您需要使用完整的TuGraph功能，请参见tugraph-db源码目录 ci/images/tugraph-runtime-*-Dockerfile，该脚本包含完整的环境构建流程。

## 2. 安装包下载

最新版本的安装包地址：

| 描述                  | 文件                                         | 链接                                                                                                                                                                                              |
|---------------------|--------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| CentOS7 安装包         | tugraph-4.5.0-1.el7.x86_64.rpm             | [下载](https://tugraph-web.oss-cn-beijing.aliyuncs.com/tugraph/tugraph-4.5.0/tugraph-4.5.0-1.el7.x86_64.rpm)                                                                                      |
| CentOS8 安装包         | tugraph-4.5.0-1.el8.x86_64.rpm             | [下载](https://tugraph-web.oss-cn-beijing.aliyuncs.com/tugraph/tugraph-4.5.0/tugraph-4.5.0-1.el8.x86_64.rpm)                                                                                      |
| Ubuntu18.04 安装包     | tugraph-4.5.0-1.x86_64.deb                 | [下载](https://tugraph-web.oss-cn-beijing.aliyuncs.com/tugraph/tugraph-4.5.0/tugraph-4.5.0-1.x86_64.deb)                                                                                          |
| CentOS7 预安装镜像       | tugraph-runtime-centos7-4.5.0.tar          | [下载](https://tugraph-web.oss-cn-beijing.aliyuncs.com/tugraph/tugraph-4.5.0/tugraph-runtime-centos7-4.5.0.tar) 、[访问](https://hub.docker.com/r/tugraph/tugraph-runtime-centos7)                   |
| CentOS8 预安装镜像       | tugraph-runtime-centos8-4.5.0.tar          | [下载](https://tugraph-web.oss-cn-beijing.aliyuncs.com/tugraph/tugraph-4.5.0/tugraph-runtime-centos8-4.5.0.tar) 、[访问](https://hub.docker.com/r/tugraph/tugraph-runtime-centos8)                   |
| Ubuntu18.04 预安装镜像   | tugraph-runtime-ubuntu18.04-4.5.0.tar      | [下载](https://tugraph-web.oss-cn-beijing.aliyuncs.com/tugraph/tugraph-4.5.0/tugraph-runtime-ubuntu18.04-4.5.0.tar) 、[访问](https://hub.docker.com/r/tugraph/tugraph-runtime-ubuntu18.04)           |
| CentOS7 精简安装包       | tugraph-mini-4.5.0-1.el7.x86_64.rpm        | [下载](https://tugraph-web.oss-cn-beijing.aliyuncs.com/tugraph/tugraph-4.5.0/tugraph-mini-4.5.0-1.el7.x86_64.rpm)                                                                                 |
| CentOS8 精简安装包       | tugraph-mini-4.5.0-1.el8.x86_64.rpm        | [下载](https://tugraph-web.oss-cn-beijing.aliyuncs.com/tugraph/tugraph-4.5.0/tugraph-mini-4.5.0-1.el8.x86_64.rpm)                                                                                 |
| Ubuntu18.04 精简安装包   | tugraph-mini-4.5.0-1.x86_64.deb            | [下载](https://tugraph-web.oss-cn-beijing.aliyuncs.com/tugraph/tugraph-4.5.0/tugraph-mini-4.5.0-1.x86_64.deb)                                                                                     |
| CentOS7 精简预安装镜像     | tugraph-mini-runtime-centos7-4.5.0.tar     | [下载](https://tugraph-web.oss-cn-beijing.aliyuncs.com/tugraph/tugraph-4.5.0/tugraph-mini-runtime-centos7-4.5.0.tar) 、[访问](https://hub.docker.com/r/tugraph/tugraph-mini-runtime-centos7)         |
| CentOS8 精简预安装镜像     | tugraph-mini-runtime-centos8-4.5.0.tar     | [下载](https://tugraph-web.oss-cn-beijing.aliyuncs.com/tugraph/tugraph-4.5.0/tugraph-mini-runtime-centos8-4.5.0.tar) 、[访问](https://hub.docker.com/r/tugraph/tugraph-mini-runtime-centos8)         |
| Ubuntu18.04 精简预安装镜像 | tugraph-mini-runtime-ubuntu18.04-4.5.0.tar | [下载](https://tugraph-web.oss-cn-beijing.aliyuncs.com/tugraph/tugraph-4.5.0/tugraph-mini-runtime-ubuntu18.04-4.5.0.tar) 、[访问](https://hub.docker.com/r/tugraph/tugraph-mini-runtime-ubuntu18.04) |
| CentOS7 编译镜像        | tugraph-compile-centos7-1.3.2.tar          | [下载](https://tugraph-web.oss-cn-beijing.aliyuncs.com/tugraph/tugraph-docker-compile/tugraph-compile-centos7-1.3.2.tar) 、[访问](https://hub.docker.com/r/tugraph/tugraph-compile-centos7)          |
| CentOS8 编译镜像        | tugraph-compile-centos8-1.3.2.tar          | [下载](https://tugraph-web.oss-cn-beijing.aliyuncs.com/tugraph/tugraph-docker-compile/tugraph-compile-centos8-1.3.2.tar) 、[访问](https://hub.docker.com/r/tugraph/tugraph-compile-centos8)          |
| Ubuntu18.04 编译镜像    | tugraph-compile-ubuntu18.04-1.3.2.tar      | [下载](https://tugraph-web.oss-cn-beijing.aliyuncs.com/tugraph/tugraph-docker-compile/tugraph-compile-ubuntu18.04-1.3.2.tar) 、[访问](https://hub.docker.com/r/tugraph/tugraph-compile-ubuntu18.04)  |
表格内容描述:
该表格包含三个列名: "描述"、"文件"、和 "链接"。

逐行描述表格中的数据内容如下：
1. CentOS7 安装包的文件为 "tugraph-4.5.0-1.el7.x86_64.rpm"，下载链接为 [下载](https://tugraph-web.oss-cn-beijing.aliyuncs.com/tugraph/tugraph-4.5.0/tugraph-4.5.0-1.el7.x86_64.rpm)。
2. CentOS8 安装包的文件为 "tugraph-4.5.0-1.el8.x86_64.rpm"，下载链接为 [下载](https://tugraph-web.oss-cn-beijing.aliyuncs.com/tugraph/tugraph-4.5.0/tugraph-4.5.0-1.el8.x86_64.rpm)。
3. Ubuntu18.04 安装包的文件为 "tugraph-4.5.0-1.x86_64.deb"，下载链接为 [下载](https://tugraph-web.oss-cn-beijing.aliyuncs.com/tugraph/tugraph-4.5.0/tugraph-4.5.0-1.x86_64.deb)。
4. CentOS7 预安装镜像的文件为 "tugraph-runtime-centos7-4.5.0.tar"，下载链接为 [下载](https://tugraph-web.oss-cn-beijing.aliyuncs.com/tugraph/tugraph-4.5.0/tugraph-runtime-centos7-4.5.0.tar)，访问链接为 [访问](https://hub.docker.com/r/tugraph/tugraph-runtime-centos7)。
5. CentOS8 预安装镜像的文件为 "tugraph-runtime-centos8-4.5.0.tar"，下载链接为 [下载](https://tugraph-web.oss-cn-beijing.aliyuncs.com/tugraph/tugraph-4.5.0/tugraph-runtime-centos8-4.5.0.tar)，访问链接为 [访问](https://hub.docker.com/r/tugraph/tugraph-runtime-centos8)。
6. Ubuntu18.04 预安装镜像的文件为 "tugraph-runtime-ubuntu18.04-4.5.0.tar"，下载链接为 [下载](https://tugraph-web.oss-cn-beijing.aliyuncs.com/tugraph/tugraph-4.5.0/tugraph-runtime-ubuntu18.04-4.5.0.tar)，访问链接为 [访问](https://hub.docker.com/r/tugraph/tugraph-runtime-ubuntu18.04)。
7. CentOS7 精简安装包的文件为 "tugraph-mini-4.5.0-1.el7.x86_64.rpm"，下载链接为 [下载](https://tugraph-web.oss-cn-beijing.aliyuncs.com/tugraph/tugraph-4.5.0/tugraph-mini-4.5.0-1.el7.x86_64.rpm)。
8. CentOS8 精简安装包的文件为 "tugraph-mini-4.5.0-1.el8.x86_64.rpm"，下载链接为 [下载](https://tugraph-web.oss-cn-beijing.aliyuncs.com/tugraph/tugraph-4.5.0/tugraph-mini-4.5.0-1.el8.x86_64.rpm)。
9. Ubuntu18.04 精简安装包的文件为 "tugraph-mini-4.5.0-1.x86_64.deb"，下载链接为 [下载](https://tugraph-web.oss-cn-beijing.aliyuncs.com/tugraph/tugraph-4.5.0/tugraph-mini-4.5.0-1.x86_64.deb)。
10. CentOS7 精简预安装镜像的文件为 "tugraph-mini-runtime-centos7-4.5.0.tar"，下载链接为 [下载](https://tugraph-web.oss-cn-beijing.aliyuncs.com/tugraph/tugraph-4.5.0/tugraph-mini-runtime-centos7-4.5.0.tar)，访问链接为 [访问](https://hub.docker.com/r/tugraph/tugraph-mini-runtime-centos7)。
11. CentOS8 精简预安装镜像的文件为 "tugraph-mini-runtime-centos8-4.5.0.tar"，下载链接为 [下载](https://tugraph-web.oss-cn-beijing.aliyuncs.com/tugraph/tugraph-4.5.0/tugraph-mini-runtime-centos8-4.5.0.tar)，访问链接为 [访问](https://hub.docker.com/r/tugraph/tugraph-mini-runtime-centos8)。
12. Ubuntu18.04 精简预安装镜像的文件为 "tugraph-mini-runtime-ubuntu18.04-4.5.0.tar"，下载链接为 [下载](https://tugraph-web.oss-cn-beijing.aliyuncs.com/tugraph/tugraph-4.5.0/tugraph-mini-runtime-ubuntu18.04-4.5.0.tar)，访问链接为 [访问](https://hub.docker.com/r/tugraph/tugraph-mini-runtime-ubuntu18.04)。
13. CentOS7 编译镜像的文件为 "tugraph-compile-centos7-1.3.2.tar"，下载链接为 [下载](https://tugraph-web.oss-cn-beijing.aliyuncs.com/tugraph/tugraph-docker-compile/tugraph-compile-centos7-1.3.2.tar)，访问链接为 [访问](https://hub.docker.com/r/tugraph/tugraph-compile-centos7)。
14. CentOS8 编译镜像的文件为 "tugraph-compile-centos8-1.3.2.tar"，下载链接为 [下载](https://tugraph-web.oss-cn-beijing.aliyuncs.com/tugraph/tugraph-docker-compile/tugraph-compile-centos8-1.3.2.tar)，访问链接为 [访问](https://hub.docker.com/r/tugraph/tugraph-compile-centos8)。
15. Ubuntu18.04 编译镜像的文件为 "tugraph-compile-ubuntu18.04-1.3.2.tar"，下载链接为 [下载](https://tugraph-web.oss-cn-beijing.aliyuncs.com/tugraph/tugraph-docker-compile/tugraph-compile-ubuntu18.04-1.3.2.tar)，访问链接为 [访问](https://hub.docker.com/r/tugraph/tugraph-compile-ubuntu18.04)。

总结：
该表格列出了不同操作系统（CentOS7、CentOS8 和 Ubuntu18.04）下与 "tugraph" 软件相关的安装包、预安装镜像和编译镜像，以及相应的下载和访问链接。共有15项内容，涵盖了完整安装、精简安装及编译镜像，方便用户根据需要进行下载和安装。


也可以访问Github进行下载：TuGraph Release(https://github.com/TuGraph-family/tugraph-db/releases)

## 3. CentOS 下的安装方法

用于在 CentOS 上安装的 TuGraph 的.rpm 安装包，其中包含了 TuGraph 可执行文件以及编写嵌入式程序和存储过程所需的头文件和相关库文件。

使用已经下载完成的`tugraph_x.y.z.rpm 安装包在终端下安装，只需要运行以下命令：

```shell
$ rpm -ivh tugraph-x.y.z.rpm
```

用户也可以通过指定`--prefix`选项指定安装目录。

## 4. Ubuntu 下的安装方法

用于在 Ubuntu 上安装的 TuGraph 的.deb 安装包，其中包含了 TuGraph 可执行文件以及编写嵌入式程序和存储过程所需的头文件和相关库文件。

使用已经下载完成的`tugraph_x.y.z.deb`安装包在终端下安装，只需要运行以下命令：

```shell
$ sudo dpkg -i tugraph-x.y.z.deb
```

该命令默认将 TuGraph 安装于`/usr/local`目录下。用户也可以通过指定 `--instdir=<directory>` 选项更改安装目录。
