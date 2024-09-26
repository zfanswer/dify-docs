# 创建知识库&上传文档

创建知识库并上传文档大致分为以下步骤**：**

1. 在 Dify 团队内创建知识库，从本地选择你需要上传的文档；
2. 选择分段与清洗模式，预览效果；
3. 配置索引方式和检索设置；
4. 等待分段嵌入；
5. 完成上传，在应用内关联并使用 🎉

以下是各个步骤的详细说明：

### 1 创建知识库

在 Dify 主导航栏中点击知识库，在该页面你可以看到团队内的知识库，点击“**创建知识库”** 进入创建向导。

* 拖拽或选中文件进行上传，批量上传的文件数量取决于[订阅计划](https://dify.ai/pricing);
* 如果还没有准备好文档，可以先创建一个空知识库;
* 如果你在创建知识库时选择了使用外部数据源（Notion 或同步 Web 站点），该知识库的类型不可更改；此举是为了防止单一知识库存在多数据源而造成的管理困难。如果你需要使用多个数据源，建议创建多个知识库并使用 [多路召回](https://docs.dify.ai/v/zh-hans/guides/knowledge-base/integrate-knowledge-within-application#duo-lu-zhao-hui-tui-jian) 模式在同一个应用内引用多个知识库。

**上传文档存在以下限制：**

* 单文档的上传大小限制为 15MB；
* SaaS 版本的不同[订阅计划](https://dify.ai/pricing)限定了**批量上传个数、文档上传总数、向量存储空间。**

<figure><img src="broken-reference" alt=""><figcaption><p>创建知识库</p></figcaption></figure>

***

### 2 选择分段与清洗策略

将内容上传至知识库后，需要先对内容进行分段与数据清洗，该阶段可以被理解为是对内容预处理与结构化。

<details>

<summary>什么是分段与清洗？</summary>

* **分段**

大语言模型存在有限的上下文窗口，无法将知识库中的所有内容发送至 LLM。因此可以将整段长文本分段处理，再基于用户问题，召回与关联度最高的段落内容，即采用分段 TopK 召回模式。此外，将用户问题与文本分段进行语义匹配时，合适的分段大小有助于找到知识库内关联性最高的文本内容，减少信息噪音。

* **清洗**

为了保证文本召回的效果，通常需要在将数据录入知识库之前便对其进行清理。例如，如果文本内容中存在无意义的字符或者空行，可能会影响问题回复的质量。关于 Dify 内置的清洗策略，详细说明请参考 [ETL](create-knowledge-and-upload-documents.md#etl)。

</details>

支持以下两种策略：

* **自动分段与清洗**
* **自定义**

{% tabs %}
{% tab title="自动分段与清洗" %}
#### 自动分段与清洗

自动模式适合对分段规则与预处理规则尚不熟悉的初级用户。在该模式下，Dify 将为你自动分段与清洗内容文件。

<figure><img src="broken-reference" alt="Automatic segmentation and cleaning"><figcaption><p>自动分段与清洗</p></figcaption></figure>
{% endtab %}

{% tab title="自定义" %}
#### 自定义

自定义模式适合对于文本处理有明确需求的进阶用户。在自定义模式下，你可以根据不同的文档格式和场景要求，手动配置文本的分段规则和清洗策略。

**分段规则：**

* **分段标识符**，指定标识符，系统将在文本中出现该标识符时分段。例如填写 `\n`（[正则表达式](https://regexr.com/)中的换行符），文本换行时将自动分段；
* **分段最大长度**，根据分段的文本字符数最大上限来进行分段，超出该长度时将强制分段。一个分段的最大长度为 4000 Tokens；
* **分段重叠长度**，分段重叠指的是在对数据进行分段时，段与段之间存在一定的重叠部分。这种重叠可以帮助提高信息的保留和分析的准确性，提升召回效果。建议设置为分段长度 Tokens 数的 10-25%；

**文本预处理规则：**文本预处理规则可以帮助过滤知识库内部分无意义的内容。

* 替换连续的空格、换行符和制表符；
* 删除所有 URL 和电子邮件地址；

<figure><img src="broken-reference" alt=""><figcaption><p>自定义</p></figcaption></figure>
{% endtab %}
{% endtabs %}

### 3 索引方式

指定内容的预处理方法（分段与清洗）后，接下来需要指定对结构化内容的索引方式。索引方式将直接影响 LLM 对知识库内容的检索效率以及回答的准确性。

系统提供以下三种索引方式，你可以根据实际需求调整每种方式内的[检索设置](create-knowledge-and-upload-documents.md#id-4-jian-suo-she-zhi)：

* 高质量
* 经济
* Q\&A 模式\


{% tabs %}
{% tab title="高质量" %}
在高质量模式下，将首先调用 Embedding 嵌入模型（支持切换）将已分段的文本转换为数字向量，帮助开发者更有效地实现大量文本信息的压缩与存储；同时还能够在用户与 LLM 对话时提供更高的准确度。

> 如需了解更多，请参考[《Embedding 技术与 Dify》](https://mp.weixin.qq.com/s/vmY\_CUmETo2IpEBf1nEGLQ)。

高质量索引方式提供向量检索、全文检索和混合检索三种检索设置。关于更多检索设置的说明，请阅读 [检索设置](create-knowledge-and-upload-documents.md#id-4-jian-suo-she-zhi)。

<figure><img src="broken-reference" alt=""><figcaption><p>高质量模式</p></figcaption></figure>
{% endtab %}

{% tab title="经济" %}
使用离线的向量引擎与关键词索引方式，降低了准确度但无需额外花费 Token，产生费用。检索方式仅提供倒排索引，详细说明请阅读[下文](create-knowledge-and-upload-documents.md#dao-pai-suo-yin)。

<figure><img src="broken-reference" alt="" width="375"><figcaption><p>经济模式</p></figcaption></figure>
{% endtab %}

{% tab title="Q&A 模式（仅社区版支持）" %}
在知识库上传文档时，系统将对文本进行分段，总结后为每分段生成 Q\&A 匹配对。与高质量与经济模式中所采用的「Q to P」（用户问题匹配文本段落）策略不同，QA 模式采用 「Q to Q」（问题匹配问题）策略。

这是因为问题文本**通常是是具有完整语法结构的自然语言**，Q to Q 的模式会令语意和匹配更加清晰，并同时满足一些高频和高相似度问题的提问场景。

当用户提问时，系统会找出与之最相似的问题，然后返回对应的分段作为答案。这种方式更加精确，因为它直接针对用户问题进行匹配，可以更准确地帮助用户检索真正需要的信息。

<figure><img src="broken-reference" alt=""><figcaption><p>Q&#x26;A 分段模式下被总结成多个 Q&#x26;A 对的文本</p></figcaption></figure>

<figure><img src="broken-reference" alt=""><figcaption><p>Q to P 与 Q to Q 的索引模式区别</p></figcaption></figure>
{% endtab %}
{% endtabs %}

***

### 4 检索设置

在**高质量索引方式**下，Dify 提供以下 3 种检索方案：

* #### **向量检索**
* **全文检索**
* **混合检索**

{% tabs %}
{% tab title="向量检索" %}
**定义：**向量化用户输入的问题并生成查询向量，比较查询向量与知识库内对应的文本向量距离，寻找最近的分段内容。

<figure><img src="broken-reference" alt=""><figcaption><p>向量检索</p></figcaption></figure>

**向量检索设置：**

**Rerank 模型：**使用第三方 Rerank 模型对向量检索召回后的分段再一次进行语义重排序，优化排序结果。在“模型供应商”页面配置 Rerank 模型的 API 秘钥之后，在检索设置中打开“Rerank 模型”。

**TopK：**用于筛选与用户问题相似度最高的文本片段。系统同时会根据选用模型上下文窗口大小动态调整片段数量。默认值为 3，数值越高，预期被召回的文本分段数量越多。

**Score 阈值：**用于设置文本片段筛选的相似度阈值，只召回超过设置分数的文本片段，默认值为 0.5。数值越高说明对于文本与问题要求的相似度越高，预期被召回的文本数量也越少。

> TopK 和 Score 设置仅在 Rerank 步骤生效，因此需要添加并开启 Rerank 模型才能应用两者中的设置。
{% endtab %}

{% tab title="全文检索" %}
**定义：**关键词检索，即索引文档中的所有词汇。用户输入问题后，通过明文关键词匹配知识库内对应的文本片段，返回符合关键词的文本片段；类似搜索引擎中的明文检索。

<figure><img src="broken-reference" alt=""><figcaption><p>全文检索</p></figcaption></figure>

**Rerank 模型：**使用第三方 Rerank 模型对全文检索召回后的分段再一次进行语义重排序，优化排序结果。在“模型供应商”页面配置 Rerank 模型的 API 秘钥之后，在检索设置中打开“Rerank 模型”。

**TopK：**用于筛选与用户问题相似度最高的文本片段。系统同时会根据选用模型上下文窗口大小动态调整片段数量。系统默认值为 3 。数值越高，预期被召回的文本分段数量越多。

**Score 阈值：**用于设置文本片段筛选的相似度阈值，只召回超过设置分数的文本片段，默认值为 0.5。数值越高说明对于文本与问题要求的相似度越高，预期被召回的文本数量也越少。

> TopK 和 Score 设置仅在 Rerank 步骤生效，因此需要添加并开启 Rerank 模型才能应用两者中的设置。
{% endtab %}

{% tab title="混合检索" %}
**定义：**同时执行全文检索和向量检索，并应用重排序步骤，从两类查询结果中选择匹配用户问题的最佳结果。在此模式下可以指定“权重设置”（无需配置 Rerank 模型 API）或选择 Rerank 模型进行检索。

<figure><img src="broken-reference" alt=""><figcaption><p>混合检索</p></figcaption></figure>

在混合检索设置内可以选择启用**“权重设置”**或**“Rerank 模型”**。

**权重设置：**允许用户赋予语义优先和关键词优先自定义的权重。关键词检索指的是在知识库内进行全文检索（Full Text Search），语义检索指的是在知识库内进行向量检索（Vector Search）。

* **语义值为 1**

仅启用语义检索模式。借助 Embedding 模型，即便知识库中没有出现查询中的确切词汇，也能通过计算向量距离的方式提高搜索的深度，返回正确内容。此外，当需要处理多语言内容时，语义检索能够捕捉不同语言之间的意义转换，提供更加准确的跨语言搜索结果。

> 语义检索指的是比对用户问题与知识库内容中的向量距离。距离越近，匹配的概率越大。参考阅读：[《Dify：Embedding 技术与 Dify 数据集设计/规划》](https://mp.weixin.qq.com/s/vmY\_CUmETo2IpEBf1nEGLQ)。

* **关键词值为 1**

仅启用关键词检索模式。通过用户输入的信息文本在知识库全文匹配，适用于用户知道确切的信息或术语的场景。该方法所消耗的计算资源较低，适合在大量文档的知识库内快速检索。

* **自定义关键词和语义权重**

除了仅启用语义检索或关键词检索模式，我们还提供了灵活的自定义权重设置。你可以通过不断调试二者的权重，找到符合业务场景的最佳权重比例。

***

**Rerank 模型：**你可以在“模型供应商”页面配置 Rerank 模型的 API 秘钥之后，在检索设置中打开“Rerank 模型”，系统会在混合检索后对已召回的文档结果再一次进行语义重排序，优化排序结果。

***

**“权重设置”**和**“Rerank 模型”**设置内支持启用以下选项：

**TopK：**用于筛选与用户问题相似度最高的文本片段。系统同时会根据选用模型上下文窗口大小动态调整片段数量。系统默认值为 3 。数值越高，预期被召回的文本分段数量越多。

**Score 阈值：**用于设置文本片段筛选的相似度阈值，即：只召回超过设置分数的文本片段。系统默认关闭该设置，即不会对召回的文本片段相似值过滤。打开后默认值为 0.5。数值越高，预期被召回的文本数量越少。
{% endtab %}
{% endtabs %}

***

在**经济索引方式**下，Dify 仅提供 1 种检索设置：

#### **倒排索引**

倒排索引是一种用于快速检索文档中关键词的索引结构，它的基本原理是将文档中的关键词映射到包含这些关键词的文档列表，从而提高搜索效率。具体原理请参考[《倒排索引》](https://zh.wikipedia.org/wiki/%E5%80%92%E6%8E%92%E7%B4%A2%E5%BC%95)。

**TopK：**用于筛选与用户问题相似度最高的文本片段。系统同时会根据选用模型上下文窗口大小动态调整片段数量。系统默认值为 3 。数值越高，预期被召回的文本分段数量越多。

<figure><img src="broken-reference" alt=""><figcaption><p>倒排索引</p></figcaption></figure>

### 5 完成上传

配置完上文所述的各项配置后，轻点“保存并处理”即可完成知识库的创建。你可以参考 [在应用内集成知识库](integrate-knowledge-within-application.md)，搭建出能够基于知识库进行问答的 LLM 应用。

***

### 参考阅读

#### ETL

在 RAG 的生产级应用中，为了获得更好的数据召回效果，需要对多源数据进行预处理和清洗，即 ETL （_extract, transform, load_）。为了增强非结构化/半结构化数据的预处理能力，Dify 支持了可选的 ETL 方案：**Dify ETL** 和[ ](https://docs.unstructured.io/welcome)[**Unstructured ETL** ](https://unstructured.io/)。Unstructured 能够高效地提取并转换您的数据为干净的数据用于后续的步骤。Dify 各版本的 ETL 方案选择：

* SaaS 版不可选，默认使用 Unstructured ETL；
* 社区版可选，默认使用 Dify ETL ，可通过[环境变量](https://docs.dify.ai/v/zh-hans/getting-started/install-self-hosted/environments#zhi-shi-ku-pei-zhi)开启 Unstructured ETL；

文件解析支持格式的差异：

| DIFY ETL                                       | Unstructured ETL                                                         |
| ---------------------------------------------- | ------------------------------------------------------------------------ |
| txt、markdown、md、pdf、html、htm、xlsx、xls、docx、csv | txt、markdown、md、pdf、html、htm、xlsx、xls、docx、csv、eml、msg、pptx、ppt、xml、epub |

不同的 ETL 方案在文件提取效果的方面也会存在差异，想了解更多关于 Unstructured ETL 的数据处理方式，请参考[官方文档](https://docs.unstructured.io/open-source/core-functionality/partitioning)。

***

**Embedding 模型**

**Embedding 嵌入**是一种将离散型变量（如单词、句子或者整个文档）转化为连续的向量表示的技术。它可以将高维数据（如单词、短语或图像）映射到低维空间，提供一种紧凑且有效的表示方式。这种表示不仅减少了数据的维度，还保留了重要的语义信息，使得后续的内容检索更加高效。

**Embedding 模型**是一种专门用于将文本向量化的大语言模型，它擅长将文本转换为密集的数值向量，有效捕捉语义信息。

> 如需了解更多，请参考：[《Dify：Embedding 技术与 Dify 数据集设计/规划》](https://mp.weixin.qq.com/s/vmY\_CUmETo2IpEBf1nEGLQ)。\
>
