# 编排节点

Chatflow 和 Workflow 类型应用内的节点均可以通过可视化拖拉拽的形式进行编排，支持**串行**和**并行**两种编排设计模式。

<figure><img src="../../.gitbook/assets/image (272).png" alt=""><figcaption><p>上图为串行节点流、下图为并行节点流</p></figcaption></figure>

## 串行设计

该结构要求节点按照预设顺序依次执行，每个节点需等待前一个节点完成并输出结果后才能开始工作，有助于**确保任务按照逻辑顺序执行。**

例如，在一个采用串行结构设计的“小说生成” AI 应用内，用户输入小说风格、节奏和角色后，LLM 按照顺序补全小说大纲、小说剧情和结尾；每个节点都基于前一个节点的输出结果展开工作，确保小说的风格一致性。

### 设计串行结构

点击两个节点中间连线的 + 号即可在中间添加一个串行节点；按照顺序将节点依次串线连接，最后将线收拢至**“结束”节点**（Workflow）/ **“直接回复”节点**（Chatflow）完成设计。

<figure><img src="../../.gitbook/assets/image (273).png" alt=""><figcaption><p>并行结构</p></figcaption></figure>

### 查看串行结构应用日志

串行结构应用内的日志将按照顺序展示各个节点的运行情况。点击对话框右上角的 「查看日志-追踪」，查看工作流完整运行过程各节点的输入 / 输出、Token 消耗、运行时长等。

<figure><img src="../../.gitbook/assets/image (275).png" alt=""><figcaption><p>并行结构应用日志</p></figcaption></figure>

## 并行设计

该设计模式允许多个节点在同一时间内共同执行，前置节点可以同时触发位于并行结构内的多个节点。并行结构内的节点不存在依赖关系，能够同时执行任务，更好地提升**节点的任务执行效率。**

例如，在某个并行设计的翻译工作流应用内，用户输入源文本触发工作流后，位于并行结构内的节点将共同收到前置节点的流转指令，同时开展多语言的翻译任务，缩短任务的处理耗时。

<figure><img src="../../.gitbook/assets/image (271).png" alt=""><figcaption><p>并行设计</p></figcaption></figure>

### 新建并行结构

你可以参考以下四种方式，通过新建节点或拖拽的方式创建并行结构。

**方式 1**

将鼠标 Hover 至某个节点，显示 `+` 按钮，支持新建多个节点，创建后自动形成并行结构。

<figure><img src="../../.gitbook/assets/image (276).png" alt=""><figcaption><p>Type 01</p></figcaption></figure>

**方式 2**

拖拽节点末尾的 `+` 按钮，拉出连线形成并行结构。

<figure><img src="../../.gitbook/assets/image (277).png" alt=""><figcaption><p>Type 02</p></figcaption></figure>

**方式 3**

如果画布存在多个节点，通过可视化拖拽的方式组成并行结构。

<figure><img src="../../.gitbook/assets/image (278).png" alt=""><figcaption><p>Type 03</p></figcaption></figure>

**方式 4**

除了在画布中通过直接添加并行节点或可视化拖拽方式组成并行结构，你也可以在节点右侧清单的“下一步”中添加并行节点，自动生成并行结构。

<figure><img src="../../.gitbook/assets/image (279).png" alt=""><figcaption><p>Type 04</p></figcaption></figure>

**Tips：**

* &#x20;画布上的“线”可以被删除；
* 并行结构的下游节点可以是任意节点；
* 在 Workflow 类型应用内需确定唯一的 “end” 节点；
* Chatflow 类型应用支持添加多个 **“直接回复”** 节点，该类型应用内的所有并行结构在末尾处均需要配置 **“直接回复”**  节点才能正常输出各个并行结构里的内容；
* 所有的并行结构都会同时运行；并行结构内的节点处理完任务后即输出结果，**输出结果时不存在顺序关系**。并行结构越简单，输出结果的速度越快。

<figure><img src="../../.gitbook/assets/image (280).png" alt=""><figcaption><p>Chatflow 应用中的并行结构</p></figcaption></figure>

### 设计并行结构应用

下文将展示四种常见的并行节点设计思路。

1. **普通并行**

普通并行指的是 `开始 | 并行结构 | 结束` 三层关系也是并行结构的最小单元。这种结构较为直观，用户输入内容后，工作流能同时执行多条任务。

> 并行分支的上限数为 10 个。

<figure><img src="../../.gitbook/assets/image (281).png" alt=""><figcaption><p>普通并行</p></figcaption></figure>

2. **嵌套并行**

嵌套并行指的是 `开始 | 多个并行结构 | 结束`多层关系，它适用于内部较为复杂的工作流，例如需要在某个节点内请求外部 API，将返回的结果同时交给下游节点处理。

一个工作流内最多支持 3 层嵌套关系。

<figure><img src="../../.gitbook/assets/image (285).png" alt=""><figcaption><p>嵌套并行：2 层嵌套关系</p></figcaption></figure>

3. **条件分支 + 并行**

并行结构也可以和条件分支共同使用。

<figure><img src="../../.gitbook/assets/image (269).png" alt=""><figcaption><p><strong>条件分支 + 并行</strong></p></figcaption></figure>

4. **迭代分支 + 并行**

迭代分支内同样支持编排并行结构，加速迭代内各节点的执行效率。

<figure><img src="../../.gitbook/assets/image (286).png" alt=""><figcaption><p>迭代分支+并行</p></figcaption></figure>

### 查看并行结构应用日志

包含并行结构的应用的运行日志支持以树状结构进行展示，你可以折叠并行节点组以更好地查看各个节点的运行日志。

<figure><img src="../../.gitbook/assets/image (287).png" alt="" width="315"><figcaption><p>并行结构</p></figcaption></figure>
