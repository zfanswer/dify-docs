# 提示词专家模式（已下线）

在 Dify 创建应用的编排默认为**简易模式**，这很适合想要快速创建应用的非技术人员，比如你想创建一个企业知识库 Chatbot 或者文章摘要生成器，利用**简易模式**编排对话前提示词，添加变量，添加上下文等简易步骤即可发布一个完整的应用（可参考👉[conversation-application.md](../../../../guides/application-orchestrate/conversation-application.md "mention")）。

而如果你是一个熟练掌握使用 **OpenAI** 的 **Playground** 的技术人员，正想创建一个学习导师应用，需要在提示词中针对不同的教学模块位置嵌入不同的上下文和变量，就可以选择**专家模式。在此模式下你可以自由地编写完整的提示词，包括修改内置的提示词，调整上下文和聊天历史内容在提示词中的位置，设定必要参数等。如果你对 Chat 和 Complete 两种模型不陌生，现在专家模式**可以快速切换 Chat 和Complete 模型以满足你的需要，并且都适用于对话型应用和文本生成型应用。

在你开始尝试新模式前，你需要知道**专家模式**下的必要元素：

*   **文本补全模型** ![](../../../../.gitbook/assets/screenshot-20231017-092613.png)

    在选择模型的时候，模型名字的右侧显示 COMPLETE 的即为文本补全模型，该类模型接受名为“提示词”的自由格式文本字符串，模型将生成一个文本补全，试图匹配您给它的任何上下文或模式。例如，如果您给的提示词：“正如笛卡尔所说，我思故”，它将高概率返回“我在”作为补全。
*   **聊天模型** <img src="../../../../.gitbook/assets/screenshot-20231017-092957.png" alt="" data-size="line">

    在选择模型的时候，模型名字的右侧显示 CHAT 的即为聊天模型，该类模型将消息列表作为输入，并返回模型生成的消息作为输出。尽管聊天格式旨在简化多轮对话，但它对于没有任何对话的单轮任务同样有用。聊天模型使用的是聊天消息作为输入和输出，包含 SYSTEM / USER / ASSISTANT 三种消息类型：

    * `SYSTEM`
      * 系统消息有助于设置 AI 助手的行为。例如，您可以修改 AI 助手的个性或提供有关它在整个对话过程中应如何表现的具体说明。系统消息是可选的，没有系统消息的模型行为可能类似于使用通用消息，例如“你是一个有帮助的助手”。
    * `USER`
      * 用户消息提供请求或评论以供 AI 助手响应。
    * `ASSISTANT`
      * 助手消息存储以前的助手响应，但也可以由您编写以提供所需行为的示例。
*   **停止序列 Stop\_Sequences**

    是指特定的单词、短语或字符，用于向 LLM 发出停止生成文本的信号。
* **专家模式提示词中的内容块**
  *   <img src="../../../../.gitbook/assets/3.png" alt="" data-size="line">

      用户在配置了数据集的 App 中，输入查询内容，App 会将查询内容作为数据集的检索条件，检索的结果在组织之后会作为上下文内容替换 `上下文` 变量，使 LLM 能够参考上下文的内容进行回答。
  *   <img src="../../../../.gitbook/assets/4.png" alt="" data-size="line">

      查询内容仅在对话型应用的文本补全模型中可用，对话中用户输入的内容将替换该变量，以触发每轮新的对话。
  *   <img src="../../../../.gitbook/assets/5.png" alt="" data-size="line">

      会话历史仅在对话型应用的文本补全模型中可用。在对话型应用中多次对话时，Dify 会将历史的对话记录根据内置规则进行组装拼接，并替换 `会话历史` 变量。其中 Human 和 Assistant 前缀可点击 `会话历史` 后的`...` 进行修改。
*   **初始模版**

    在**专家模式**下，正式编排之前，提示词框会给到一个初始模版，我们可以直接修改初始模版来对 LLM有更加定制化的要求。注意：不同类型应用的不同类型模式下有所区别。

    具体请参考👉[prompt-engineering-template.md](prompt-engineering-template.md "mention")

## 两种模式对比

<table><thead><tr><th width="333">对比维度</th><th width="197">简易模式</th><th>专家模式</th></tr></thead><tbody><tr><td>内置提示词可见性</td><td>封装不可见</td><td>开放可见</td></tr><tr><td>有无自动编排</td><td>可用</td><td>不可用</td></tr><tr><td>文本补全模型和聊天模型选择有无区别</td><td>无</td><td>文本补全模型和聊天模型选择后有编排区别</td></tr><tr><td>变量插入</td><td>有</td><td>有</td></tr><tr><td>内容块校验</td><td>无</td><td>有</td></tr><tr><td>SYSTEM / USER / ASSISTANT<br>三种消息类型编排</td><td>无</td><td>有</td></tr><tr><td>上下文参数设置</td><td>可设置</td><td>可设置</td></tr><tr><td>查看 PROMPT LOG</td><td>可查看完整提示词日志</td><td>可查看完整提示词日志</td></tr><tr><td>停止序列 Stop_Sequences 参数设置</td><td>无</td><td>可设置</td></tr></tbody></table>

## 操作说明

### 1. 如何进入专家模式

创建应用后，在提示词编排页可以切换至**专家模式**，在此模式下可以编辑完整的应用提示词。

<figure><img src="../../../../.gitbook/assets/专家模式.png" alt=""><figcaption><p>专家模式入口</p></figcaption></figure>

{% hint style="warning" %}
在**专家模式**下修改提示词并发布应用后，将无法返回至**简易模式**。
{% endhint %}

### 2. 修改插入上下文参数

在**简易模式**和**专家模式**下，都可以对插入上下文的参数进行修改，参数包含了 **TopK** 和 **Score 阈值**。

{% hint style="warning" %}
需要注意的是，我们只有先上传了上下文，在**专家模式**下才会呈现包含 \{{#context#\}} 的内置提示词
{% endhint %}

<figure><img src="../../../../.gitbook/assets/参数设置.png" alt=""><figcaption><p>上下文参数设置</p></figcaption></figure>

**TopK：值范围为整数 1～10**

用于筛选与用户问题相似度最高的文本片段。系统同时会根据选用模型上下文窗口大小动态调整片段数量。系统默认值为 2 。这个值建议可以设置为 2～5 ，因为我们期待的是得到与嵌入的上下文匹配度更高的答案。

**Score 阈值：值范围为两位小数的浮点数 0～1**

用于设置文本片段筛选的相似度阈值，即：只召回超过设置分数的文本片段（在“命中测试”中我们可以查看到每个片段的命中分数）。系统默认关闭该设置，即不会对召回的文本片段相似值过滤。打开后默认值为 0.7 。这里我们推荐保持默认关闭设置，如果你有更精准的回复要求，也可以设置更高的值（最高值为1，不建议过高）

### 3. 设置**停止序列 Stop\_Sequences**

我们不期望 LLM 生成多余的内容，所以需要设置指特定的单词、短语或字符(默认设置为 `Human:`)，告知 LLM 停止生成文本。

比如你在提示词中写了 _Few-Shot_:

```
Human1: 天是什么颜色

Assistant1: 天是蓝色的

Human1: 火是什么颜色

Assistant1: 火是红色的

Human1: 土是什么颜色

Assistant1: 
```

那么在模型参数里的 `停止序列 Stop_Sequences`，输入 `Human1:`，并按下 "Tab" 键。

这样 LLM 在回复的时候只会回复一句：

```
Assistant1: 土是黄色的
```

而不会生成多余的对话（即 LLM 生成内容到达下一个 “Human1:” 之前就停止了）。

### 4. 快捷插入变量和内容块

在**专家模式**下，你可以在文本编辑器中输入“`/`”，快捷调出内容块来插入提示词中。内容块分为：`上下文`、`变量`、`会话历史`、`查询内容`。你也可以通过输入“`{`”，快捷插入已创建过的变量列表。\\

<figure><img src="../../../../.gitbook/assets/快捷键.png" alt=""><figcaption><p>快捷键 “/”</p></figcaption></figure>

{% hint style="warning" %}
除“`变量`”以外的其他内容块不可重复插入。在不同应用和模型下，可插入的内容块会根据不同的提示词模板结构有所区别，`会话历史`、`查询内容` 仅在对话型应用的文本补全模型中可用。
{% endhint %}

### 5. 输入对话前提示词

系统的提示词初始模版提供了必要的参数和 LLM 回复要求，详情见👉：[prompt-engineering-template.md](prompt-engineering-template.md "mention")。

而开发人员前期编排的核心是对话前提示词（`Pre-prompt`），需要编辑后插入内置提示词，建议的插入位置如下（以创建 “iPhone 咨询客服”为例）：

```
When answer to user:
- If you don't know, just say that you don't know.
- If you don't know when you are not sure, ask for clarification.
Avoid mentioning that you obtained the information from the context.
And answer according to the language of the user's question.

你是 Apple 公司的一位客服助手，你可以为用户提供 iPhone 的咨询服务。
当你回答时需要列出 iPhone 详细参数，你必须一定要把这些信息输出为竖向 MARKDOWN 表格，若列表过多则进行转置。
你被允许长时间思考从而生成更合理的输出。
注意：你目前掌握的只是一部分 iPhone 型号，而不是全部。
```

当然，你也可以定制化修改提示词初始模版，比如你希望 LLM 回复的语言都是英文，你可以将上述的内置提示词修改为：

```
When answer to user:
- If you don't know, just say that you don't know.
- If you don't know when you are not sure, ask for clarification.
Avoid mentioning that you obtained the information from the context.
And answer according to the language English.
```

### 6. 调试日志

编排调试时不仅可以查看用户的输入和 LLM 的回复。在**专家模式**下，点击发送消息左上角图标，可以看到完整的提示词，方便开发者确认输入变量内容、上下文、聊天记录和查询内容是否符合预期。日志列表的相关说明请查看日志文档 👉 ： [logs.md](../../../../guides/biao-zhu/logs.md "mention")

#### 6.1 **查看调试日志**

在调试预览界面，用户与 AI 产生对话之后，将鼠标指针移动到任意的用户会话，即可在左上角看到“日志”标志按钮，点击即可查看提示词日志。

<figure><img src="../../../../.gitbook/assets/日志.png" alt=""><figcaption><p>调试日志入口</p></figcaption></figure>

在日志中，我们可以清晰的查看到：

* 完整的内置提示词
* 当前会话引用的相关文本片段
* 历史会话记录

<figure><img src="../../../../.gitbook/assets/11.png" alt=""><figcaption><p>调试预览界面查看提示词日志</p></figcaption></figure>

从日志中，我们可以查看经过系统拼装后最终发送至 LLM 的完整提示词，并根据调试结果持续改进提示词输入。

#### **6.2 追溯调试历史**

在初始的构建应用主界面，左侧导航栏可以看到“日志与标注”，点击进去即可查看完整的日志。 在日志与标注的主界面，点击任意一个会话日志条目，在弹出的右侧对话框中同样鼠标指针移动到会话上即可点开“日志”按钮查看提示词日志。

<figure><img src="../../../../.gitbook/assets/12.png" alt=""><figcaption><p>日志与标注界面查看提示词日志</p></figcaption></figure>
