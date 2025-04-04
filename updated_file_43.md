# 使用 TuGraph 图学习模块进行点分类

## 1.简介
GNN 是许多图上机器学习任务的强大工具。在本介绍性教程中，您将学习使用 GNN 进行点分类的基本工作流程，即预测图中点的类别。

此文档将展示如何构建一个 GNN 用于在 Cora 数据集上仅使用少量标签进行半监督点分类，这是一个以论文为点、引文为边的引文网络。任务是预测给定论文的类别。

通过完成本教程，您可以

使用 TuGraph 加载 cora 数据集。

使用 TuGraph 提供的采样算子采样，并构建 GNN 模型。

在 CPU 或 GPU 上训练用于点分类的 GNN 模型。

此文档需要对图神经网络、DGL等使用有一定经验。

## 2. 前置条件

TuGraph图学习模块需要TuGraph-db 3.5.1及以上版本。

TuGraph部署推荐采用Docker镜像tugraph-compile 1.2.4及以上版本:

tugraph / tugraph-compile-ubuntu18.04:latest

tugraph / tugraph-compile-centos7:latest

tugraph / tugraph-compile-centos8:latest

以上镜像均可在DockerHub上获取。

## 3. Cora 数据集导入TuGraph数据库
### 3.1. Cora 数据集介绍
Cora 数据集由 2708 篇论文组成，分为 7 个类别。每篇论文由一个 1433 维的词袋表示，表示论文中的单词是否出现。这些词袋特征已经预处理，以从 0 到 1 的范围归一化。边表示论文之间的引用关系。

TuGraph中已经提供了Cora数据集的导入工具，用户可以直接使用。

### 3.2. 数据导入
Cora数据集在test/integration/data/algo目录下，包含点集cora_vertices和边集cora_edge。

首先需要将Cora数据集导入到TuGraph数据库中。

在build/output目录下执行：
```bash
cp -r ../../test/integration/data/ ./ && cp -r ../../learn/examples/* ./
```

该指令将数据集相关文件拷贝到build/output目录下。

然后进行数据导入：
```bash
./lgraph_import -c ./data/algo/cora.conf --dir ./coradb --overwrite 1
```
其中cora.conf为图schema文件，代表图数据的格式,可参考test/integration/data/algo/cora.conf。coradb为导入后的图数据文件名称，代表图数据的存储位置。

## 4. feature特征转换
由于Cora数据集中的feature特征为长度为1433的float类型数组，TuGraph暂不支持float数组类型加载，因此可将其按照string类型导入后，转换成char*方便后续存取，具体实现可参考feature_float.cpp文件。
具体执行过程如下：

在build目录下编译导入plugin(如果TuGraph已编译可跳过)：
`make feature_float_embed`

在build/output目录下执行
`./algo/feature_float_embed ./coradb`
即可进行转换。

## 5. 编译采样算子
采样算子用于从数据库中获取图数据并转换成所需数据结构，具体执行过程如下(如果TuGraph已编译，可跳过此步骤)：
在tugraph-db/build文件夹下执行
`make -j2`

或在tugraph-db/learn/procedures文件夹下执行
`python3 setup.py build_ext -i`

```python
from lgraph_db_python import *  # 导入tugraph-db的python接口模块
import importlib  # 导入importlib模块
getdb = importlib.import_module("getdb")  #获取getdb算子
getdb.Process(db, olapondb, feature_len, NodeInfo, EdgeInfo) #调用getdb算子
```

如代码所示，得到算子so文件后，import 导入使用。

## 6. 模型训练及保存
TuGraph在python层调用cython层的算子，实现图学习模型的训练。
使用 TuGraph 图学习模块使用方式介绍如下:
在build/output文件夹下执行
`python3 train_full_cora.py --model_save_path ./cora_model`
即可进行训练。
最终打印loss数值小于0.9，即为训练成功。至此，图模型训练完成，模型保存在cora_model文件。

训练详细过程如下：
### 6.1.数据加载
```python
galaxy = PyGalaxy(args.db_path)
galaxy.SetCurrentUser(args.username, args.password)
db = galaxy.OpenGraph(args.graph_name, False)
```
如代码所示，根据图数据路径、用户名、密码和子图名称将数据加载到内存中。TuGraph可以载入多个子图用于图训练，在此处我们只载入一个子图。

### 6.2.构建采样器
训练过程中，首先使用GetDB算子从数据库中获取图数据并转换成所需数据结构，具体代码如下：
```python
    GetDB.Process(db_: lgraph_db_python.PyGraphDB, olapondb: lgraph_db_python.PyOlapOnDB, feature_num: size_t, NodeInfo: list, EdgeInfo: list)
```
如代码所示，结果存储在NodeInfo和EdgeInfo中。NodeInfo和EdgeInfo是python list结果，其存储的信息结果如下：

| 图数据 | 存储信息位置 |
| --- | --- |
| 边起点 | EdgeInfo[0] |
| 边终点 | EdgeInfo[1] |
| 顶点ID | NodeInfo[0] |
| 顶点特征 | NodeInfo[1] |
| 顶点标签 | NodeInfo[2] |
表格内容描述: 
此表格包含两列，分别为“图数据”和“存储信息位置”。 

- 第一行显示边的起点，存储在EdgeInfo数组的第0个索引位置。
- 第二行显示边的终点，存储在EdgeInfo数组的第1个索引位置。
- 第三行为顶点ID，存储在NodeInfo数组的第0个索引位置。
- 第四行显示顶点特征，存储在NodeInfo数组的第1个索引位置。
- 第五行为顶点标签，存储在NodeInfo数组的第2个索引位置。

总结: 此表格详细列出了图数据中的边和顶点信息，并指明了各项数据在相应存储位置的索引，便于理解数据的结构和存储方式。

然后构建采样器
```python
    batch_size = 5
    count = 2708
    sampler = TugraphSample(args)
    dataloader = dgl.dataloading.DataLoader(fake_g,
        torch.arange(count),
        sampler,
        batch_size=batch_size,
        num_workers=0,
        )
```

### 6.3.对结果进行格式转换
```python
    src = EdgeInfo[0].astype('int64')
    dst = EdgeInfo[1].astype('int64')
    nodes_idx = NodeInfo[0].astype('int64')
    remap(src, dst, nodes_idx)
    features = NodeInfo[1].astype('float32')
    labels = NodeInfo[2].astype('int64')
    g = dgl.graph((src, dst))
    g.ndata['feat'] = torch.tensor(features)
    g.ndata['label'] = torch.tensor(labels)
    return g
```
对结果进行格式转换，使之符合训练格式

### 6.4.构建GCN模型
```python
class GCN(nn.Module):
    def __init__(self, in_size, hid_size, out_size):
        super().__init__()
        self.layers = nn.ModuleList()
        # two-layer GCN
        self.layers.append(dgl.nn.GraphConv(in_size, hid_size, activation=F.relu))
        self.layers.append(dgl.nn.GraphConv(hid_size, out_size))
        self.dropout = nn.Dropout(0.5)

    def forward(self, g, features):
        h = features
        for i, layer in enumerate(self.layers):
            if i != 0:
                h = self.dropout(h)
            h = layer(g, h)
        return h

def build_model():
    in_size = feature_len  #feature_len为feature的长度，在此处为1433
    out_size = classes  #classes为类别数，在此处为7
    model = GCN(in_size, 16, out_size)  #16为隐藏层大小
    return model
```
本教程将构建一个两层图卷积网络（GCN）。每层通过聚合邻居信息来计算新的点表示。

### 6.5.训练GCN模型
```python
loss_fcn = nn.CrossEntropyLoss()
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
    if model_save_path != "":   #如果需要保存模型，则给出模型保存路径
        if 'min_loss' not in train.__dict__:
            train.min_loss = current_loss
        elif current_loss < train.min_loss:
            train.min_loss = current_loss
            model_save_path = 'best_model.pth'
        torch.save(model.state_dict(), model_save_path)
    return current_loss

for epoch in range(50):
    model.train()
    total_loss = 0
    loss = train(g, model)
    if epoch % 5 == 0:
        print('In epoch', epoch, ', loss', loss)
    sys.stdout.flush()
```
如代码所示，根据定义好的采样器、优化器和模型进行迭代训练50次，训练后的模型保存至model_save_path路径中。

输出结果如下：
```bash
In epoch 0 , loss 1.9586775302886963
In epoch 5 , loss 1.543689250946045
In epoch 10 , loss 1.160698413848877
In epoch 15 , loss 0.8862786889076233
In epoch 20 , loss 0.6973256468772888
In epoch 25 , loss 0.5770673751831055
In epoch 30 , loss 0.5271289348602295
In epoch 35 , loss 0.45514997839927673
In epoch 40 , loss 0.43748989701271057
In epoch 45 , loss 0.3906335234642029
```

同时，图学习模块可采用GPU进行加速，用户如果需要再GPU上运行，需要用户自行安装相应的GPU驱动和环境。具体可参考learn/README.md。

完整代码可参考learn/examples/train_full_cora.py
