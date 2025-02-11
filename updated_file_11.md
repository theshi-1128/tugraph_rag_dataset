# Heterogeneous Graph

> 本文档介绍如何使用异质图进行训练。

## 1. 异质图简介
异质图（Heterogeneous Graph）是指由不同类型的节点和边构成的图结构。在异质图中，节点和边可以具有多样化的属性和关系，代表了不同实体以及它们之间的复杂关联。

在异质图中，节点类型可以代表不同的实体，如用户、商品、话题等，而边类型表示不同实体之间的关系，如用户之间的关注关系、用户与商品之间的购买关系等。节点和边可以具有不同的属性。

异质图提供了一种强大的图模型，能够更好地表达和分析具有多种类型实体和复杂关系的现实世界系统。在不同领域的数据分析和应用中，异质图具有广泛的应用前景和研究价值。

## 2. 异质图创建
在TuGraph中，一个异构图由一系列边关系构成。每个关系由一个字符串三元组定义 (源节点类型, 边类型, 目标节点类型) 。异质图的创建方式与同质图类似，只是在创建图时需要指定字符串三元组定义。如下所示。
    
```python
    olapondb = PyOlapOnDB('Empty', db, txn, [("node", "edge", "node")])
```
其中，第四个参数为异质图的边关系定义，可以通过该参数，指定筛选的异质图点边类型。如果不指定该参数，则默认将全部点边类型的数据进行构图训练。

## 3. 异质图查询接口
为了方便用户使用，当用户给定第四个参数时，TuGraph提供了查询异质图点边类型的接口。示例如下所示：

### 3.1 点类型查询接口
```python
olapondb.ntypes()
```
返回值为点类型列表，如['node1', 'node2', 'node3']。

### 3.2 边类型查询接口
```python
olapondb.etypes()
```
返回值为边类型列表，如['edge1', 'edge2', 'edge3']。

### 3.3 点类型和边类型查询接口
```python
olapondb.metagraph()
```
返回值为字符串三元组定义 (源节点类型, 边类型, 目标节点类型)，如
[('node1', 'edge1', 'node2'), ('node2', 'edge2', 'node3')]。

## 4. 异质图输出格式
和同质图相同的是，异质图的采样数据结果也存储在NodeInfo和EdgeInfo中。
可通过如下方式获取输出数据。
```python
    NodeInfo = []
    EdgeInfo = []
    getdb.Process(db, olapondb, feature_len, NodeInfo, EdgeInfo)
```
其中getdb为获取全图数据的函数，db为图数据库实例，olapondb为图分析类。feature_len为节点特征长度，NodeInfo和EdgeInfo为输出的节点和边信息。

其存储信息结果如下：
| 图数据 | 存储信息位置 |
| --- | --- |
| 边起点 | EdgeInfo[0] |
| 边终点 | EdgeInfo[1] |
| 边类型 | EdgeInfo[2] |
| 顶点ID | NodeInfo[0] |
| 顶点特征 | NodeInfo[1] |
| 顶点标签 | NodeInfo[2] |
| 顶点类型 | NodeInfo[3] |
表格内容描述: 该表格包含两列，分别为“图数据”和“存储信息位置”。

逐行描述表格中的数据内容：
1. 第一行显示边的起点存储在EdgeInfo[0]。
2. 第二行显示边的终点存储在EdgeInfo[1]。
3. 第三行显示边的类型存储在EdgeInfo[2]。
4. 第四行显示顶点的ID存储在NodeInfo[0]。
5. 第五行显示顶点的特征存储在NodeInfo[1]。
6. 第六行显示顶点的标签存储在NodeInfo[2]。
7. 第七行显示顶点的类型存储在NodeInfo[3]。

总体总结: 表格提供了图数据的不同要素及其对应的存储位置，分别涉及边的起点、终点和类型，以及顶点的ID、特征、标签和类型，为图数据的存储结构提供了清晰的概述。

## 5. 异质图训练
异构图训练的目标是学习图中节点和边的表示，以便于进行后续的任务，如节点分类、链接预测、图聚类等。为了实现这一目标，研究者们提出了多种基于图神经网络（Graph Neural Networks，GNNs）的模型。这些模型通过聚合邻居节点的信息来更新节点的表示，进而捕捉图结构中的复杂关系。

由于异构图中包含多种类型的节点和边，因此在设计GNN模型时需要考虑如何处理这些不同类型的信息。一种常见的方法是设计不同的聚合函数来分别处理不同类型的邻居节点。此外，还需要考虑如何将这些不同类型的信息整合到一起，以便于模型能够有效地学习到节点和边的表示。

TuGraph 提供了使用裁剪版ogbn-mag数据集进行异质图训练的方法，可供使用者参考。

TuGraph提供的官方docker中暂未提供异质图训练的环境，因此需要用户自行安装相关依赖包。
在训练之前需要下载ogb和pandas包，具体安装方式如下：
```shell
pip3 install ogb
pip3 install pandas==0.24.2
```

训练代码如下所示：

```python
def train(graph, model, model_save_path):
    optimizer = torch.optim.Adam(model.parameters(), lr=1e-2, weight_decay=5e-4)
    model.train()
    s = time.time()
    load_time = time.time()
    graph = dgl.add_self_loop(graph)
    logits = model(graph, graph.ndata['feat'])
    loss = loss_fcn(logits, graph.ndata['label'])
    optimizer.zero_grad()
    loss.backward()
    optimizer.step()
    train_time = time.time()
    current_loss = float(loss)
    if model_save_path != "":
        if 'min_loss' not in train.__dict__:
            train.min_loss = current_loss
        elif current_loss < train.min_loss:
            train.min_loss = current_loss
            model_save_path = 'best_model.pth'
        torch.save(model.state_dict(), model_save_path)
    return current_loss
```
全部训练代码可参考tugraph/learn/examples/train_full_mag.py文件。
