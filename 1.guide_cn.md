# 文档地图
这里是文档地图，帮助用户快速学习和使用TuGraph Analytics。

## 介绍
**TuGraph Analytics** (别名：GeaFlow) 是蚂蚁集团开源的性能世界一流(https://ldbcouncil.org/benchmarks/snb-bi/)的OLAP图数据库，支持万亿级图存储、图表混合处理、实时图计算、交互式图分析等核心能力，目前广泛应用于数仓加速、金融风控、知识图谱以及社交网络等场景。

GeaFlow设计论文参考：[GeaFlow: A Graph Extended and Accelerated Dataflow System](https://dl.acm.org/doi/abs/10.1145/3589771)

## 快速上手

1. 准备Git、JDK8、Maven、Docker环境。
2. 下载源码：`git clone https://github.com/TuGraph-family/tugraph-analytics`
3. 项目构建：`mvn clean install -DskipTests`
4. 测试任务：`./bin/gql_submit.sh --gql geaflow/geaflow-examples/gql/loop_detection.sql`
3. 构建镜像：`./build.sh --all`
4. 启动容器：`docker run -d --name geaflow-console -p 8888:8888 geaflow-console:0.1`

## 开发手册

GeaFlow支持DSL和API两套编程接口（DSL应用开发和API应用开发），您既可以通过GeaFlow提供的类SQL扩展语言SQL+ISO/GQL进行流图计算作业的开发，也可以通过GeaFlow的高阶API编程接口通过Java语言进行应用开发。

## 实时能力

相比传统的流式计算引擎比如Flink、Storm这些以表为模型的实时处理系统而言，GeaFlow以图为数据模型，在处理Join关系运算，尤其是复杂多跳的关系运算如3跳以上的Join、复杂环路查找上具备极大的性能优势。

## 合作伙伴

华中科技大学、大数据技术与系统国家地方联合工程研究中心、服务计算技术与系统教育部重点实验室、集群与网格计算湖北省重点实验室、复旦大学知识工场实验室、浙江大学数据库与大数据分析实验室、WhaleOps、OceanBase、SecretFlow隐语。


