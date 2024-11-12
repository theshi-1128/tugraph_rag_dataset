# Traversal API介绍
GeaFlow对外提供了实现图遍历算法的接口，通过实现该接口进行子图遍历，全图遍历。用户可在遍历算法中选取点边继续遍历，并定义迭代次数。

## 动态图
### 接口
| API | 接口说明 | 入参说明 |
| --- | --- | --- |
| void open(IncVertexCentricTraversalFuncContext<K, VV, EV, M, R> vertexCentricFuncContext) | vertexCentricFunction进行open操作 | vertexCentricFuncContext：K表示vertexId的类型，VV表示vertex value类型，EV表示edge value类型，M表示图遍历中定义的消息类型，R表示遍历结果类型。 |
| void init(ITraversalRequest traversalRequest) | 图遍历初始化接口 | traversalRequest：图遍历触发点，其中K表示vertex id的类型。 |
| void evolve(K vertexId, TemporaryGraph<K, VV, EV> temporaryGraph) | 首轮计算对增量图实现处理逻辑 | vertexId：当前计算点的id，其中K表示vertex id的类型。<br>temporaryGraph：临时增量图，其中K表示vertexId的类型，VV表示vertex value类型，EV表示edge value类型。 |
| void compute(K vertexId, Iterator messageIterator) | 图遍历接口 | vertexId：当前计算点的id，其中K表示vertex id的类型。<br>messageIterator：图遍历过程中所有发送给当前vertex的消息，其中M表示遍历迭代过程中定义的发送消息类型。 |
| void finish(K vertexId, MutableGraph<K, VV, EV> mutableGraph) | 图遍历完成接口 | vertexId：当前计算点的id，其中K表示vertex id的类型。<br>mutableGraph：可变图，其中K表示vertexId的类型，VV表示vertex value类型，EV表示edge value类型。 |
表格内容描述:

该表格包含三列，分别为“API”、“接口说明”和“入参说明”。

1. 第一行数据描述：
   - API: `void open(IncVertexCentricTraversalFuncContext<K, VV, EV, M, R> vertexCentricFuncContext)`
   - 接口说明: 该接口用于对vertexCentricFunction进行open操作。
   - 入参说明: `vertexCentricFuncContext`包含五种类型参数，其中K表示vertexId的类型，VV表示vertex value类型，EV表示edge value类型，M表示图遍历中定义的消息类型，R表示遍历结果类型。

2. 第二行数据描述：
   - API: `void init(ITraversalRequest traversalRequest)`
   - 接口说明: 这是图遍历的初始化接口。
   - 入参说明: `traversalRequest`为图遍历的触发点，其中K表示vertex id的类型。

3. 第三行数据描述：
   - API: `void evolve(K vertexId, TemporaryGraph<K, VV, EV> temporaryGraph)`
   - 接口说明: 该接口实现对增量图的首轮计算处理逻辑。
   - 入参说明: `vertexId`表示当前计算点的id（K表示vertex id的类型），`temporaryGraph`是一个临时增量图，包含vertexId（K）、vertex value（VV）和edge value（EV）的类型信息。

4. 第四行数据描述：
   - API: `void compute(K vertexId, Iterator messageIterator)`
   - 接口说明: 此接口用于图遍历过程。
   - 入参说明: `vertexId`为当前计算点的id（K表示vertex id的类型），`messageIterator`是发送给当前vertex的消息迭代器，其中M表示遍历过程中的消息类型。

5. 第五行数据描述：
   - API: `void finish(K vertexId, MutableGraph<K, VV, EV> mutableGraph)`
   - 接口说明: 该接口用于标记图遍历的完成。
   - 入参说明: `vertexId`表示当前计算点的id（K表示vertex id的类型），`mutableGraph`为可变图，包含vertexId（K）、vertex value（VV）和edge value（EV）的类型信息。

总体上，表格描述了一系列用于图遍历操作的API，包括初始化、增量计算、消息传递及完成状态。这些API的入参具有详细的类型指示，从而便于开发者理解如何使用这些接口进行图的遍历和处理。


- 详细接口

```java
   public interface IncVertexCentricTraversalFunction<K, VV, EV, M, R> extends IncVertexCentricFunction<K, VV
   , EV, M> {

   void open(IncVertexCentricTraversalFuncContext<K, VV, EV, M, R> vertexCentricFuncContext);

   void init(ITraversalRequest<K> traversalRequest);

   void evolve(K vertexId, TemporaryGraph<K, VV, EV> temporaryGraph);

   void compute(K vertexId, Iterator<M> messageIterator);

   void finish(K vertexId, MutableGraph<K, VV, EV> mutableGraph);

   interface IncVertexCentricTraversalFuncContext<K, VV, EV, M, R> extends IncGraphContext<K, VV, EV,
   M> {
   /** 激活遍历起点用以下一轮迭代使用 */
   void activeRequest(ITraversalRequest<K> request);
   /** 收集遍历结果 */
   void takeResponse(ITraversalResponse<R> response);

        void broadcast(IGraphMessage<K, M> message);
    	/** 获取历史图数据 */
        TraversalHistoricalGraph<K, VV, EV> getHistoricalGraph();
   }


    interface TraversalHistoricalGraph<K, VV, EV>  extends HistoricalGraph<K, VV, EV> {
    	/** 获取指定版本快照 */
        TraversalGraphSnapShot<K, VV, EV> getSnapShot(long version);
    }

    interface TraversalGraphSnapShot<K, VV, EV> extends GraphSnapShot<K, VV, EV> {
    	/** 获取开始图遍历的点 */
        TraversalVertexQuery<K, VV> vertex();
    	/** 获取开始图遍历的边 */
        TraversalEdgeQuery<K, EV> edges();
    }
}
```

### 示例

```java
public class IncrGraphTraversalAll {

    private static final Logger LOGGER =
        LoggerFactory.getLogger(IncrGraphTraversalAll.class);
    
    public static void main(String[] args) {
        Environment environment = EnvironmentFactory.onLocalEnvironment();
        Pipeline pipeline = PipelineFactory.buildPipeline(environment);
        String graphName = "graph_view_name";
        GraphViewDesc graphViewDesc = GraphViewBuilder.createGraphView(graphName)
            .withShardNum(2)
            .withBackend(BackendType.RocksDB)
            .withSchema(new GraphMetaType(IntegerType.INSTANCE, ValueVertex.class, Integer.class, ValueEdge.class, IntegerType.class))
            .build();
        pipeline.withView(graphName, graphViewDesc);
        pipeline.submit(new PipelineTask() {
            @Override
            public void execute(IPipelineTaskContext pipelineTaskCxt) {
                PWindowSource<IVertex<Integer, Integer>> vertices =
                    pipelineTaskCxt.buildSource(new RecoverableFileSource<>("data/input/email_edge",
                            line -> {
                                String[] fields = line.split(",");
                                IVertex<Integer, Integer> vertex1 = new ValueVertex<>(
                                    Integer.valueOf(fields[0]), 1);
                                IVertex<Integer, Integer> vertex2 = new ValueVertex<>(
                                    Integer.valueOf(fields[1]), 1);
                                return Arrays.asList(vertex1, vertex2);
                            }), SizeTumblingWindow.of(10000));
                
                PWindowSource<IEdge<Integer, Integer>> edges =
                    pipelineTaskCxt.buildSource( new RecoverableFileSource<>("data/input/email_edge",
                        line -> {
                            String[] fields = line.split(",");
                            IEdge<Integer, Integer> edge = new ValueEdge<>(Integer.valueOf(fields[0]),
                                Integer.valueOf(fields[1]), 1);
                            return Collections.singletonList(edge);
                        }), SizeTumblingWindow.of(5000));

                PGraphView<Integer, Integer, Integer> fundGraphView =
                    pipelineTaskCxt.getGraphView(graphName);
                PIncGraphView<Integer, Integer, Integer> incGraphView =
                    fundGraphView.appendGraph(vertices, edges);
                incGraphView.incrementalTraversal(new IncGraphTraversalAlgorithms(3))
                    .start()
                    .sink(v -> {});
            }
        });
        IPipelineResult result = pipeline.execute();
        result.get();
    }
    
    public static class IncGraphTraversalAlgorithms extends IncVertexCentricTraversal<Integer,
            Integer, Integer, Integer, Integer> {
        
        public IncGraphTraversalAlgorithms(long iterations) {
            super(iterations);
        }
        
        @Override
        public IncVertexCentricTraversalFunction<Integer, Integer, Integer, Integer, Integer> getIncTraversalFunction() {
            return new IncVertexCentricTraversalFunction<Integer, Integer, Integer, Integer, Integer>() {

                private IncVertexCentricTraversalFuncContext<Integer, Integer, Integer, Integer, Integer> vertexCentricFuncContext;

                @Override
                public void open(IncVertexCentricTraversalFuncContext<Integer, Integer, Integer, Integer,
                    Integer> vertexCentricFuncContext) {
                    this.vertexCentricFuncContext = vertexCentricFuncContext;
                }

                @Override
                public void evolve(Integer vertexId,
                                   TemporaryGraph<Integer, Integer, Integer> temporaryGraph) {
                    MutableGraph<Integer, Integer,
                        Integer> mutableGraph = this.vertexCentricFuncContext.getMutableGraph();
                    IVertex<Integer, Integer> vertex = temporaryGraph.getVertex();
                    if (vertex != null) {
                        mutableGraph.addVertex(0, vertex);
                    }
                    List<IEdge<Integer, Integer>> edges = temporaryGraph.getEdges();
                    if (edges != null) {
                        for (IEdge<Integer, Integer> edge : edges) {
                            mutableGraph.addEdge(0, edge);
                        }
                    }
                }

                @Override
                public void init(ITraversalRequest<Integer> traversalRequest) {
                    int requestId = traversalRequest.getVId();
                    List<IEdge<Integer, Integer>> edges =
                        this.vertexCentricFuncContext.getHistoricalGraph().getSnapShot(0).edges().getEdges();
                    int sum = 0;
                    if (edges != null) {
                        for (IEdge<Integer, Integer> edge : edges) {
                            sum += edge.getValue();
                        }
                    }
                    this.vertexCentricFuncContext.takeResponse(new TraversalResponse(requestId, sum));
                }

                @Override
                public void compute(Integer vertexId, Iterator<Integer> messageIterator) {
                }

                @Override
                public void finish(Integer vertexId,
                                   MutableGraph<Integer, Integer, Integer> mutableGraph) {
                }
            };
        }
        @Override
        public VertexCentricCombineFunction<Integer> getCombineFunction() {
            return null;
        }
    }

    static class TraversalResponse implements ITraversalResponse<Integer> {

        private long responseId;

        private int value;

        public TraversalResponse(long responseId, int value) {
            this.responseId = responseId;
            this.value = value;
        }
        @Override
        public long getResponseId() {
            return responseId;
        }
        @Override
        public Integer getResponse() {
            return value;
        }
        @Override
        public ResponseType getType() {
            return ResponseType.Vertex;
        }
        @Override
        public String toString() {
            return responseId + "," + value;
        }
    }
}
```

## 静态图

### 接口
| API | 接口说明 | 入参说明 |
| --- | --- | --- |
| void open(VertexCentricTraversalFuncContext<K, VV, EV, M, R> vertexCentricFuncContext) | vertexCentric function进行open操作 | vertexCentricFuncContext：K表示vertexId的类型，VV表示vertex value类型，EV表示edge value类型，M表示图遍历中定义的消息类型，R表示遍历结果类型。 |
| void init(ITraversalRequest traversalRequest) | 图遍历初始化接口 | traversalRequest：图遍历触发点，其中K表示vertex id的类型。 |
| void compute(K vertexId, Iterator messageIterator) | 图遍历接口 | vertexId：当前计算点的id，其中K表示vertex id的类型。<br>messageIterator：图遍历过程中所有发送给当前vertex的消息，其中M表示遍历迭代过程中定义的发送消息类型。 |
表格内容描述:

该表格包含三列：API、接口说明和入参说明。

1. 第一行：
   - API: `void open(VertexCentricTraversalFuncContext<K, VV, EV, M, R> vertexCentricFuncContext)`
   - 接口说明: 进行 vertexCentric function 的 open 操作。
   - 入参说明: `vertexCentricFuncContext` 包含多个类型参数，K 表示顶点 ID 的类型，VV 表示顶点值类型，EV 表示边值类型，M 表示图遍历中定义的消息类型，R 表示遍历结果类型。

2. 第二行：
   - API: `void init(ITraversalRequest traversalRequest)`
   - 接口说明: 用于图遍历的初始化接口。
   - 入参说明: `traversalRequest` 指定图遍历的触发点，其中 K 表示顶点 ID 的类型。

3. 第三行：
   - API: `void compute(K vertexId, Iterator messageIterator)`
   - 接口说明: 实现图遍历的接口。
   - 入参说明: `vertexId` 表示当前计算点的 ID，其中 K 为顶点 ID 的类型；`messageIterator` 包含在图遍历过程中所有发送给当前顶点的消息，其类型为 M，表示遍历迭代过程中定义的消息类型。

总体而言，此表格详细列出了图遍历相关的 API 接口及其说明，包括对每个 API 的功能描述和输入参数的具体要求。


- 详细接口

```java
public interface VertexCentricTraversalFunction<K, VV, EV, M, R> extends VertexCentricFunction<K, VV
    , EV, M> {

    void open(VertexCentricTraversalFuncContext<K, VV, EV, M, R> vertexCentricFuncContext);
	/** 图遍历算法初始化方法 */
    void init(ITraversalRequest<K> traversalRequest);
	/** 实现图遍历逻辑 */
    void compute(K vertexId, Iterator<M> messageIterator);

    void finish();

    void close();
	
    interface VertexCentricTraversalFuncContext<K, VV, EV, M, R> extends VertexCentricFuncContext<K,
        VV, EV, M> {
    	/** 获取图遍历结果 */
        void takeResponse(ITraversalResponse<R> response);
    	/** 获取开始图遍历的点 */
        TraversalVertexQuery<K, VV> vertex();
    	/** 获取开始图遍历的边 */
        TraversalEdgeQuery<K, EV> edges();

        void broadcast(IGraphMessage<K, M> message);
    }

    interface TraversalVertexQuery<K, VV> extends VertexQuery<K, VV> {
    	/** 获取图遍历中点的迭代器 */
        Iterator<K> loadIdIterator();
    }

    interface TraversalEdgeQuery<K, EV> extends EdgeQuery<K, EV> {
    	/** 通过指定的点id，获取对应的图遍历起点 */
        TraversalEdgeQuery<K, EV> withId(K vertexId);
    }
}
```

### 示例

```java
public class StaticGraphTraversalAllExample {
    private static final Logger LOGGER =
            LoggerFactory.getLogger(StaticGraphTraversalAllExample.class);

    public static void main(String[] args) {
        Environment environment = EnvironmentFactory.onLocalEnvironment();
        Pipeline pipeline = PipelineFactory.buildPipeline(environment);
        pipeline.submit(new PipelineTask() {
            @Override
            public void execute(IPipelineTaskContext pipelineTaskCxt) {
                PWindowSource<IVertex<Integer, Integer>> prVertices =
                        pipelineTaskCxt.buildSource(new FileSource<>("data/input/email_vertex",
                                line -> {
                                    String[] fields = line.split(",");
                                    IVertex<Integer, Integer> vertex = new ValueVertex<>(Integer.valueOf(fields[0]),
                                            Integer.valueOf(fields[1]));
                                    return Collections.singletonList(vertex);
                                }), AllWindow.getInstance()).withParallelism(1);

                PWindowSource<IEdge<Integer, Integer>> prEdges =
                        pipelineTaskCxt.buildSource(new FileSource<>("data/input/email_edge",
                                line -> {
                                    String[] fields = line.split(",");
                                    IEdge<Integer, Integer> edge = new ValueEdge<>(Integer.valueOf(fields[0]),
                                            Integer.valueOf(fields[1]), 1);
                                    return Collections.singletonList(edge);
                                }), AllWindow.getInstance()).withParallelism(1);

                GraphViewDesc graphViewDesc = GraphViewBuilder
                        .createGraphView(GraphViewBuilder.DEFAULT_GRAPH)
                        .withShardNum(1)
                        .withBackend(BackendType.Memory)
                        .build();

                PGraphWindow<Integer, Integer, Integer> graphWindow =
                        pipelineTaskCxt.buildWindowStreamGraph(prVertices, prEdges, graphViewDesc);

                graphWindow.traversal(new VertexCentricTraversal<Integer, Integer, Integer, Integer, Integer>(3) {
                    @Override
                    public VertexCentricTraversalFunction<Integer, Integer, Integer, Integer,
                            Integer> getTraversalFunction() {
                        return new VertexCentricTraversalFunction<Integer, Integer, Integer, Integer, Integer>() {

                            private VertexCentricTraversalFuncContext<Integer, Integer, Integer, Integer, Integer> vertexCentricFuncContext;

                            @Override
                            public void open(
                                    VertexCentricTraversalFuncContext<Integer, Integer, Integer, Integer, Integer> vertexCentricFuncContext) {
                                this.vertexCentricFuncContext = vertexCentricFuncContext;
                            }

                            @Override
                            public void init(ITraversalRequest<Integer> traversalRequest) {
                                this.vertexCentricFuncContext.takeResponse(
                                        new TraversalResponse(traversalRequest.getRequestId(), 1));
                            }
                            @Override
                            public void compute(Integer vertexId, Iterator<Integer> messageIterator) {
                            }
                            @Override
                            public void finish() {
                            }
                            @Override
                            public void close() {
                            }
                        };
                    }

                    @Override
                    public VertexCentricCombineFunction<Integer> getCombineFunction() {
                        return null;
                    }
                }).start().sink(v -> {});
            }
        });

        IPipelineResult result = pipeline.execute();
        result.get();
    }
    public static class TraversalResponse implements ITraversalResponse<Integer> {
        private long responseId;
        private int response;
        public TraversalResponse(long responseId, int response) {
            this.responseId = responseId;
            this.response = response;
        }

        @Override
        public long getResponseId() {
            return responseId;
        }

        @Override
        public Integer getResponse() {
            return response;
        }

        @Override
        public ResponseType getType() {
            return ResponseType.Vertex;
        }

        @Override
        public String toString() {
            return "TraversalResponse{" + "responseId=" + responseId + ", response=" + response
                    + '}';
        }
    }

}
```
