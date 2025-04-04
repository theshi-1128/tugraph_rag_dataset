# 技术解读 | TuGraph 图分析引擎技术剖析

图分析引擎又称图计算框架，主要用于进行复杂图分析，是一种能够全量数据集运行快速循环迭代的技术，适用场景包括社区发现、基因序列预测、重要性排名等，典型算法有 PageRank、WCC、BFS、LPA、SSSP。

TuGraph 图数据管理平台社区版已于 2022 年 9 月在 Github 开源，本文将对 TuGraph 图分析引擎的技术进行剖析。

## 1 TuGraph 图分析引擎概览

TuGraph 的图分析引擎面向的场景主要是全图/全量数据分析类的任务。借助 TuGraph 的 C++ 图分析引擎 API，用户可以对不同数据来源的图数据快速导出一个待处理的复杂子图，然后在该子图上运行诸如 BFS、PageRank、LPA、WCC 等迭代式图算法，最后根据运行结果做出相应的对策。在 TuGraph 中，导出和计算过程均可以通过在内存中并行处理的方式进行加速，从而达到近乎实时的处理分析。与传统方法相比，即避免了数据导出落盘的开销，又能使用紧凑的图数据结构获得计算的理想性能。

根据数据来源及实现不同，可分为 **Procedure**、**Embed** 和 **Standalone** 三种运行模式。其中，Procedure 模式和 Embed 模式的数据源是图存储中加载图数据，分别适用于 Client/Server 部署，以及服务端直接调用，后者多用于调试。

Standalone 模式的数据源是 TXT、二进制、ODPS 文件等外部数据源，能够独立于图数据存储直接运行分析算法。

TuGraph 图计算系统社区版内置 6 个基础算法，商业版内置了共 34 种算法，涵盖了图结构、社区发现、路径查询、重要性分析、模式挖掘和关联性分析的六大类常用方法，可以满足多种业务场景需要，因此用户几乎不需要自己实现具体的图计算过程。

| 算法类型               | 中文算法名                      | 英文算法名                           | 程序名                   |
|----------------------|-----------------------------|-----------------------------------|-----------------------|
| 路径查询               | 广度优先搜索                  | Breadth-First Search              | bfs                   |
|                       | 单源最短路径                  | Single-Source Shortest Paths      | sssp                  |
|                       | 全对最短路径                  | All-Pair Shortest Path            | apsp                  |
|                       | 多源最短路径                  | Multiple-source Shortest Paths     | mssp                  |
|                       | 两点间最短路径                | Single-Pair Shortest Paths        | psp                   |
| 重要性分析            | 网页排序                       | Pagerank                          | pagerank              |
|                       | 介数中心度                    | Betweenness Centrality            | bc                    |
|                       | 置信度传播                    | Belief Propagation                | bp                    |
|                       | 距离中心度                    | Closeness Centrality              | clc                   |
|                       | 个性化网页排序                | Personalized PageRank             | ppr                   |
|                       | 带权重的网页排序              | Weighted Pagerank Algorithm       | wpagerank             |
|                       | 信任指数排名                  | Trustrank                         | trustrank             |
|                       | 超链接主题搜索                | Hyperlink-Induced Topic Search    | hits                  |
| 关联性分析            | 平均集聚系数                  | Local Clustering Coefficient      | lcc                   |
|                       | 共同邻居                      | Common Neighborhood                | cn                    |
|                       | 度数关联度                    | Degree Correlation                 | dc                    |
|                       | 杰卡德系数                    | Jaccard Index                      | ji                    |
| 图结构                | 直径估计                      | Dimension Estimation               | de                    |
|                       | K核算法                      | K-core                             | kcore                 |
|                       | k阶团计数算法                | Kcliques                          | kcliques              |
|                       | k阶桁架计数算法              | Ktruss                            | ktruss                |
|                       | 最大独立集算法                | Maximal independent set           | mis                   |
| 社区发现              | 弱连通分量                    | Weakly Connected Components        | wcc                   |
|                       | 标签传播                      | Label Propagation Algorithm       | lpa                   |
|                       | EgoNet算法                   | EgoNet                            | en                    |
|                       | 鲁汶社区发现                  | Louvain                           | louvain               |
|                       | 强连通分量                    | Strongly Connected Components      | scc                   |
|                       | 监听标签传播                  | Speaker-listener Label Propagation | lpa                   |
|                       | 莱顿算法                      | Leiden                            | leiden                |
|                       | 带权重的标签传播              | Weighted Label Propagation Algorithm | wlpa                  |
| 模式挖掘              | 三角计数                      | Triangle Counting                  | triangle              |
|                       | 子图匹配算法                  | Subgraph Isomorphism              | subgraph_isomorphism   |
|                       | 模式匹配算法                  | Motif                             | motif                 |
表格内容描述: 
该表格包含四列，分别是“算法类型”、“中文算法名”、“英文算法名”和“程序名”。根据这些列名，可以看出表格主要是列出不同算法的种类及其对应的名称和程序标识。

- 第一行的“路径查询”下有五种算法：
  1. 广度优先搜索（Breadth-First Search），程序名为bfs
  2. 单源最短路径（Single-Source Shortest Paths），程序名为sssp
  3. 全对最短路径（All-Pair Shortest Path），程序名为apsp
  4. 多源最短路径（Multiple-source Shortest Paths），程序名为mssp
  5. 两点间最短路径（Single-Pair Shortest Paths），程序名为psp

- 第二行的“重要性分析”下有八种算法：
  1. 网页排序（Pagerank），程序名为pagerank
  2. 介数中心度（Betweenness Centrality），程序名为bc
  3. 置信度传播（Belief Propagation），程序名为bp
  4. 距离中心度（Closeness Centrality），程序名为clc
  5. 个性化网页排序（Personalized PageRank），程序名为ppr
  6. 带权重的网页排序（Weighted Pagerank Algorithm），程序名为wpagerank
  7. 信任指数排名（Trustrank），程序名为trustrank
  8. 超链接主题搜索（Hyperlink-Induced Topic Search），程序名为hits

- 第三行的“关联性分析”下有四种算法：
  1. 平均集聚系数（Local Clustering Coefficient），程序名为lcc
  2. 共同邻居（Common Neighborhood），程序名为cn
  3. 度数关联度（Degree Correlation），程序名为dc
  4. 杰卡德系数（Jaccard Index），程序名为ji

- 第四行的“图结构”下有五种算法：
  1. 直径估计（Dimension Estimation），程序名为de
  2. K核算法（K-core），程序名为kcore
  3. k阶团计数算法（Kcliques），程序名为kcliques
  4. k阶桁架计数算法（Ktruss），程序名为ktruss
  5. 最大独立集算法（Maximal independent set），程序名为mis

- 第五行的“社区发现”下有八种算法：
  1. 弱连通分量（Weakly Connected Components），程序名为wcc
  2. 标签传播（Label Propagation Algorithm），程序名为lpa
  3. EgoNet算法（EgoNet），程序名为en
  4. 鲁汶社区发现（Louvain），程序名为louvain
  5. 强连通分量（Strongly Connected Components），程序名为scc
  6. 监听标签传播（Speaker-listener Label Propagation），程序名为lpa
  7. 莱顿算法（Leiden），程序名为leiden
  8. 带权重的标签传播（Weighted Label Propagation Algorithm），程序名为wlpa

- 第六行的“模式挖掘”下有三种算法：
  1. 三角计数（Triangle Counting），程序名为triangle
  2. 子图匹配算法（Subgraph Isomorphism），程序名为subgraph_isomorphism
  3. 模式匹配算法（Motif），程序名为motif

总结：该表格全面列出了多种图算法，分为路径查询、重要性分析、关联性分析、图结构、社区发现及模式挖掘六大类，涵盖了各种算法的中文名、英文名及其对应的程序标识，便于读者了解和选择合适的算法。


## 2 功能介绍

### 2.1 图分析框架

图分析框架作为图分析引擎的“骨架”，可以联合多种模块有效的耦合协同工作。一般分为预处理、算法过程、结果分析三个阶段。

预处理部分用于读入数据及参数进行图构建及相关信息的存储统计，并整理出算法过程所需的参数及数据。

算法过程会根据得到的数据通过特定的算法进行逻辑计算，并得到结果数据。 结果分析部分根据得到的结果数据进行个性化处理（如取最值等），并将重要的信息写回和打印输出操作。

### 2.2 点边筛选器

点边筛选器作用于图分析引擎中的 Procedure 和 Embed 模式。对于图存储数据源可根据用户需要和实际业务场景对图数据进行筛查，选择有效的点边进行图结构的构建。

### 2.3 一致性快照

TuGraph 中的 Procedure 和 Embed 模式能够提供数据“快照”，即建立一个对指定数据集的完全可用拷贝，该拷贝包括相应数据在某个时间点（拷贝开始的时间点）的镜像。由于 OLAP 的操作仅涉及读操作而不涉及写操作，OlapOnDB 会以一种更紧凑的方式对数据进行排布，在节省空间的同时，提高数据访问的局部性。

### 2.4 块状读写模块

块状读写模块作用于图分析引擎中的 Standalone 模式，用于对不同外部数据源的数据进行高效读入，同时也包含对内部算法处理后的图数据结果写回。

### 2.5 参数模块

参数模块作用于分析引擎中的 Standalone 模式，用于对图的一般信息（如数据来源、算法名称、数据输入、输出路径、顶点个数等）以及根据不同数据来源、不同算法所配置的不同信息参数进行接受和整理，传输给图算法及各个模块，同时将最终结果模块化展示。

## 3 使用示例

由前文所述可知，图分析引擎分为 Standalone、Embed 和 Procedure 模式，现在以 BFS 算法为例分别介绍它们的使用方式。

### 3.1 Procedure 模式

Procedure 模式主要用于 Client/Server 的 TuGraph 运行时，图算法的加载和调用。在 TuGraph/plugins 目录下执行以下命令即可在 TuGraph/plugins 目录下得到 bfs.so 文件：

```bash
bash make_so.sh bfs
```

将该文件以插件形式上传至 TuGraph-web，输入参数后即可执行。

#### 示例

在 TuGraph/plugins 编译.so 算法文件

```bash
bash make_so.sh bfs
```

将 bfs.so 文件以插件形式加载至 TuGraph-web 后，输入如下 JSON 参数：

```json
{
	"root_id": "0",
	"label": "node",
	"field": "id"
}
```

即可得到返回结果。

```json
{
  "core_cost": 0.013641119003295898,
  "found_vertices": 3829,
  "num_edges": 88234,
  "num_vertices": 4039,
  "output_cost": 8.821487426757813e-06,
  "prepare_cost": 0.03479194641113281,
  "total_cost": 0.04844188690185547
}
```

#### 输出内容解释：
- `num_edges`：表示该图数据的边数量
- `num_vertices`：表示该图数据顶点的数量
- `prepare_cost`：表示预处理阶段所需要的时间。
- `core_cost`：表示算法运行所需要的时间。
- `found_vertices`：表示查找到顶点的个数。
- `output_cost`：表示算法结果写回 db 所需要的时间。
- `total_cost`：表示执行该算法整体运行时间。

### 3.2 Embed 模式

该种方式主要用于TuGraph在后台程序中对预加载的图存储数据进行算法分析，多用于快速调试。在TuGraph/plugins目录下对embed_main.cpp文件完善，补充数据名称、输入参数、数据路径等信息，示例如下：

#### C++ 示例代码

以下是一个使用 TuGraph 数据库的 C++ 示例代码，展示如何打开数据库并处理请求。

```cpp
#include <iostream>
#include "lgraph/lgraph.h"
#include "lgraph/olap_base.h"

using namespace std;

extern "C" bool Process(lgraph_api::GraphDB &db, const std::string &request, std::string &response);

int main(int argc, char **argv) {
    // db_path表示预加载图数据存放的路径
    std::string db_path = "./fb_db/";
    
    if (argc > 1) {
        db_path = argv[1];
    }
    
    lgraph_api::Galaxy g(db_path);
    g.SetCurrentUser("admin", "730TuGraph");
    // 指定图数据的名称
    lgraph_api::GraphDB db = g.OpenGraph("fb_db");
    
    std::string resp;
    // 以json形式输入算法参数
    bool r = Process(db, "{\"root_id\": \"0\", \"label\": \"node\", \"field\": \"id\"}", resp);
    
    cout << r << endl;
    cout << resp << endl;
    
    return 0;
}


保存后在TuGraph/plugins目录下执行 bash make_so.sh bfs 即可在TuGraph/plugins/cpp目录下的到bfs_procedure文件，bash make_embed.sh bfs

在TuGraph/plugins文件夹下执行./cpp/bfs_procedure即可得到返回结果。

```json
{"root_id":"0", "label":"node", "field":"id"}
```

found_vertices = 3829

```json
{
    "core_cost": 0.025603055953979492,
    "found_vertices": 3829,
    "num_edges": 88234,
    "num_vertices": 4039,
    "output_cost": 9.059906005859375e-06,
    "prepare_cost": 0.056738853454589844,
    "total_cost": 0.0823509693145752
}
```

### 3.3 Standalone 模式

Standalone 模式可以独立于图存储运行，直接从文本文件或 ODPS 读取 Edgelist 形式的图数据。在 TuGraph/build 目录下执行 make bfs_standalone 即可得到 bfs_standalone 文件,该文件生成与 TuGraph/build/output/algo 文件夹下。运行：在 TuGraph/build 目录下执行./output/algo/bfs_standalone -–type [type] –-input_dir [input_dir] -–vertices [vertices] --root [root] –-output_dir [output_dir]
	•[type]：表示输入图文件的类型来源，包含 text 文本文件、BINARY_FILE 二进制文件和 ODPS 源。
	•[input_dir]：表示输入图文件的文件夹路径，文件夹下可包含一个或多个输入文件。TuGraph 在读取输入文件时会读取[input_dir]下的所有文件，要求[input_dir]下只能包含输入文件，不能包含其它文件。参数不可省略。
	•[vertices]：表示图的顶点个数，为 0 时表示用户希望系统自动识别顶点数量；为非零值时表示用户希望自定义顶点个数，要求用户自定义顶点个数需大于最大的顶点 ID。参数可省略，默认值为 0。
	•[root]：表示进行 bfs 的起始顶点 id。参数不可省略。
	•[output_dir]：表示输出数据保存的文件夹路径，将输出内容保存至该文件中，参数不可省略。

示例：在 TuGraph/build 编译 standalone 算法程序

make bfs_standalone

在 TuGraph/build/output 目录下运行 text 源文件

/output/algo/bfs_standalone --type text --
input_dir ../test/integration/data/algo/fb_unweighted --root 0

得到运行结果：

- **prepare_cost**: 0.10 s
- **core_cost**: 0.02 s
- **found_vertices**: 3829
- **output_cost**: 0.00 s
- **total_cost**: 0.11 s
**DONE.**


结果参数解释同上。

## 4 小结

综上，图分析引擎可以高效、快速地处理多种来源的数据，其并行的图构建方式保证了内存占用小的特点。此外，图分析引擎也具有易于安装部署、灵活性高、耦合程度低、易于上手等对用户友好的特性，可以帮助用户结合具体业务解决问题。

