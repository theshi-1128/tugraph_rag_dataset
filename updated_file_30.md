# Sink API介绍
GeaFlow对外提供了Sink API，用于构建Window Sink，用户可以通过实现SinkFunction来定义具体的输出逻辑。

## 接口
| API | 接口说明 | 入参说明 |
| -------- | -------- | -------- |
| PStreamSink<T> sink(SinkFunction<T> sinkFunction)     | 将结果进行输出。     |sinkFunction：用户可以通过实现SinkFunction接口，用于定义其相应的输出语义。GeaFlow内部集成了几种sink function，例如：Console、File等。|
表格内容描述：该表格包含三列，分别是“API”、“接口说明”和“入参说明”。

第一行数据中，API为“PStreamSink<T> sink(SinkFunction<T> sinkFunction)”，接口说明为“将结果进行输出。”，入参说明为“sinkFunction：用户可以通过实现SinkFunction接口，用于定义其相应的输出语义。GeaFlow内部集成了几种sink function，例如：Console、File等。”

整体来说，该表格描述了一个输出接口的功能及其参数的详细信息，主要提供了如何通过实现SinkFunction接口来定义数据输出方式，并提及了内置的多种输出方式。
* Sink用法
```java
	// 将结果直接打印到console。
	source.sink(v -> {LOGGER.info("result: {}", v)});
```

## 示例
```java
public class WindowStreamWordCount {

    private static final Logger LOGGER = LoggerFactory.getLogger(WindowStreamWordCount.class);

    public static void main(String[] args) {
        Environment environment = EnvironmentFactory.onLocalEnvironment();
        Pipeline pipeline = PipelineFactory.buildPipeline(environment);
        pipeline.submit(new PipelineTask() {
            @Override
            public void execute(IPipelineTaskContext pipelineTaskCxt) {
                Configuration config = pipelineTaskCxt.getConfig();
                List<String> words = Lists.newArrayList("hello", "world", "hello", "word");
                PWindowSource<String> source = pipelineTaskCxt.buildSource(new CollectionSource<String>(words) {
                }, SizeTumblingWindow.of(100));
                // 将结果直接打印在console
                source.sink(v -> {
                    LOGGER.info("result: {}", v);
                });
            }
        });

        IPipelineResult result = pipeline.execute();
        result.get();
    }
}
```
