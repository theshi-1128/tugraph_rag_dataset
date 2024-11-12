# FAQ

在 TuGraph 使用过程中，如果遇到问题可优先查看用户文档。其次，请查看如下 FAQ 列表是否能够匹配您的问题。

## 内核引擎问题汇总

### Q: TuGraph 的边是否支持索引？
A: TuGraph 在引擎层支持边索引，可通过存储过程使用。Cypher的边索引功能正在开发支持中。

### Q: TuGraph 单机的QPS是多少？
A: 不同数据规模，不同查询操作的QPS差异较大，比如LDBC SNB典型图操作超过1.2万。


## TuGraph Browser问题汇总

### Q: 可视化文件 build 后如何更新到 tugraph 服务？
A: 可视化文件打包后，需要进行以下操作进行替换。
- 登录 tugraph 服务所在的服务或 docker 容器内。
- 通过 `lgraph_server --help` 查看服务启动的配置文件所在目录。通常情况：`/usr/local/etc/lgraph.json`。
- 查看 `/usr/local/etc/lgraph.json` 文件中 web 的配置目录。通常情况：`/usr/local/share/lgraph/resource`。
- 将可视化打包后生成的文件夹中的内容全部替换到配置目录下 `/usr/local/share/lgraph/resource`。
- 重新启动 tugraph 服务。

### Q: 如何通过 npm run dev，连接已有的 tugraph 服务？
A: 启动之前，需要修改文件 `.env.development` 中的 `VUE_APP_REQUESTURL` 的配置项。然后在通过 `npm run dev` 进行启动。
示例：
```
NODE_ENV=development 
VUE_APP_TITLE=TuGraph(dev) 
VUE_APP_REQUESTURL=http://localhost:7070/
```

### Q: 3.3.0 版本 TuGraph Browser 删除 node label 失败？
A: 可视化建模部分删除node label报错的问题，已经在https://github.com/TuGraph-db/tugraph-web/tree/v3.3.1仓库完成修复。


## 客户端问题汇总

### Q: client 目前有哪些编程语言，是否支持 node js？
A: 目前主要支持的编程语言有 c++, python, java；目前不支持 node js。使用 node 作为主要开发语言的用户，可以使用 tugraph 提供的 restful api 来调用。建议使用 Cypher 来封装调用接口。后续版本 restful api 将不再进行更新维护，只会保留登录、登出、刷新 token、cypher 调用这几个常见的 api。

### Q: python client 是否支持 pip install？client 在哪里进行引用？
A: 目前 python client 不支持 pip 进行安装。client 在目录 https://github.com/TuGraph-db/tugraph-db/tree/master/src/client


## 数据导入问题汇总

### Q: TuGraph 可以对接哪些常用数据库？
A: TuGraph通过DataX可以实现大部分主流数据库的导入导出，支持的数据库包括MySQL、Oracle、Hive 等。

### Q：如何进行数据导入？
A：可以使用lgraph_import批量导入工具将现有数据导入 TuGraph。lgraph_import支持从 CSV 文件和 JSON 数据源导入数据。TuGraph 支持两种导入模式：1、离线模式：读取数据并将其导入指定服务器的数据文件，应仅在服务器离线时完成。 2、在线模式：读取数据并将其发送到工作中的服务器，然后将数据导入其数据库。


## 存储过程问题汇总

### Q: 如何加载存储过程或算法包？
A: 加载方式有两种：
- 第一种：通过可视化页面的插件模块，通过交互操作完成加载。
- 第二种：通过 cypher 语句实现存储过程的加载。
```
CALL db.plugin.loadPlugin(plugin_type::STRING, plugin_name::STRING, plugin_content::STRING, code_type::STRING, plugin_description::STRING, read_only::BOOLEAN) :: (::VOID)
```

### Q: 如何调用或执行存储过程？
A: 可以使用 cypher 进行存储过程的执行或调用。
```
CALL db.plugin.callPlugin(plugin_type::STRING, plugin_name::STRING, param::STRING, timeout::DOUBLE, in_process::BOOLEAN) :: (success::BOOLEAN, result::STRING)
```

### Q: 开源内置的算法包在哪里？
A: 算法包代码地址 https://github.com/TuGraph-db/tugraph-db/tree/master/plugins

## 安装部署问题汇总

### Q: 如何使用 docker 镜像安装？
A:
- 确认本地是否有 docker 环境，可使用 `docker -v` 进行验证。如果没有请安装 docker，安装方式见 docker 官网文档 https://docs.docker.com/install/
- 下载 docker 镜像，下载方式可使用 `docker pull tugraph/tugraph-runtime-centos7`，也可以在官网下载页面进行下载 https://www.tugraph.org/download [注：下载的文件是 *.tar.gz 的压缩包，不用解压]。
- 如果使用 `docker pull` 下载的镜像则不用导入镜像。如果使用官网下载的压缩包，则要使用 `docker load -i ./tugraph_x.y.z.tar` [注：x.y.z 是版本号的代替符，具体数值根据自己下载的版本进行改写]。
- 启动 docker 容器：
```
docker run -d -p 7070:7070 -p 9090:9090 --name tugraph_demo tugraph/tugraph-runtime-centos7 lgraph_server
```
[注：具体的镜像名称 tugraph/tugraph-runtime-centos7 要以本地实际镜像名称为准，可用过 `docker images` 命令查看]。

### Q: rpm 包和 deb 包安装后，启动 lgraph_server 服务。提示缺少 'liblgraph.so' 报错？
A: 此问题主要是环境变量导致，需要配置环境变量。
示例：
```
export LD_LIBRARY_PATH=/usr/local/lib64
```

### Q: 如何在 Mac 的 M1 芯片上编译 TuGraph？
A: 请参考 https://zhuanlan.zhihu.com/p/561139698 中的教程来在Mac 的 M1 芯片上编译 TuGraph。

### Q: 安装报错 jemalloc: Unsupported system page size 该如何处理？
A: 
1. 重新编译并重新安装 jemalloc 库。
2. 使用新的 jemalloc 库重新构建 TuGraph 二进制文件:
```
cmake .. -DCMAKE_BUILD_TYPE=Release -DENABLE_BUILD_ON_AARCH64=ON
```

## Cypher问题汇总

### Q: 是否支持不定长边的条件查询？
示例：
```
MATCH p=(v)-[e:acted_in|:rate*1..3]-(v2) WHERE id(v) IN [3937] AND e.stars = 3 RETURN p
```
A: 目前还不支持不定长边的过滤查询。目前的代替方案只能是分开写。上面的示例，就需要从 1 跳到 3 跳都写一遍。

### Q: 如何查询最短路径，shortestPath 函数如何使用？
A: 使用示例如下（示例图谱：MovieDemo）
```
MATCH (n1 {name:'Corin Redgrave'}),(n2 {name:'Liam Neeson'})
CALL algo.allShortestPaths(n1,n2) YIELD nodeIds,relationshipIds,cost
RETURN nodeIds,relationshipIds,cost
```
详尽使用方案请参考官网文档。

### Q: 查询语句 Where 后使用 and 进行拼接查询速度较慢，语句应如何优化改进？
示例：
```
MATCH (n1),(n2) CALL algo.allShortestPaths(n1,n2)
YIELD nodeIds,relationshipIds,cost
WHERE id(n1) IN [0] AND id(n2) IN [3938]
RETURN nodeIds,relationshipIds,cost
```
A: 目前 cypher 查询引擎正在优化中。现阶段语句改写可以通过 with 向下传递进行优化。
示例：
```
MATCH (n1) WHERE id(n1) IN [0] WITH n1
MATCH (n2) WHERE id(n2) IN [3938] WITH n1, n2
CALL algo.allShortestPaths(n1,n2) YIELD nodeIds,relationshipIds,cost
RETURN nodeIds,relationshipIds,cost
```

### Q: 如何查询任意跳的边？
A: 使用 *..  
示例：
```
MATCH p=(a)-[*..]-(b) WHERE id(a) IN [3] AND id(b) IN [19] RETURN p
```

### Q: Cypher 是否支持 date()函数？
A: date()函数还没有实现，我们将在之后实现它。

## Jwt Token问题汇总

### Q: 报错“User has reached the maximum number of tokens”后该如何处理？
A: 这表明当前账号 Token 数量已达上限 10000 个。解决方法如下，任选其一：
1. 登出不使用的 Token。
2. 重新启动 TuGraph 服务，会清空所有 Token。
3. Token 有效期默认为 24 小时，24 小时后会自动失效并删除。



## 如果问题仍然无法得到解决，我们推荐您在 https://github.com/TuGraph-family/tugraph-db/discussions 跟帖或发起新贴来描述您的问题。同时 TuGraph QA 大汇总也会根据大家在讨论区发起的帖子，进行定期的更新。
