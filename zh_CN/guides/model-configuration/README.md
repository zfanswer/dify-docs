# 模型

Dify 是基于大语言模型的 AI 应用开发平台，初次使用时你需要先在 Dify 的 **设置 -- 模型供应商** 页面内添加并配置所需要的模型。

<figure><img src="../../.gitbook/assets/image (165).png" alt=""><figcaption><p>设置-模型供应商</p></figcaption></figure>

Dify 目前已支持主流的模型供应商，例如 OpenAI 的 GPT 系列、Anthropic 的 Claude 系列等。不同模型的能力表现、参数类型会不一样，你可以根据不同情景的应用需求选择你喜欢的模型供应商。**你在 Dify 应用以下模型能力前，应该前往不同的模型厂商官方网站获得他们的 API key 。**

### 模型类型

在 Dify 中，我们按模型的使用场景将模型分为以下 4 类：

1.  **系统推理模型**。 在创建的应用中，用的是该类型的模型。智聊、对话名称生成、下一步问题建议用的也是推理模型。

    > 已支持的系统推理模型供应商：[OpenAI](https://platform.openai.com/account/api-keys)、[Azure OpenAI Service](https://azure.microsoft.com/en-us/products/ai-services/openai-service/)、[Anthropic](https://console.anthropic.com/account/keys)、Hugging Face Hub、Replicate、Xinference、OpenLLM、[讯飞星火](https://www.xfyun.cn/solutions/xinghuoAPI)、[文心一言](https://console.bce.baidu.com/qianfan/ais/console/applicationConsole/application)、[通义千问](https://dashscope.console.aliyun.com/api-key\_management?spm=a2c4g.11186623.0.0.3bbc424dxZms9k)、[Minimax](https://api.minimax.chat/user-center/basic-information/interface-key)、ZHIPU(ChatGLM)
2.  **Embedding 模型**。在数据集中，将分段过的文档做 Embedding 用的是该类型的模型。在使用了数据集的应用中，将用户的提问做 Embedding 处理也是用的该类型的模型。

    > 已支持的 Embedding 模型供应商：OpenAI、ZHIPU(ChatGLM)、Jina AI
3.  [**Rerank 模型**](https://docs.dify.ai/v/zh-hans/advanced/retrieval-augment/rerank)。**Rerank 模型用于增强检索能力，改善 LLM 的搜索结果。**

    > 已支持的 Rerank 模型供应商：Cohere、Jina AI
4.  **语音转文字模型**。将对话型应用中，将语音转文字用的是该类型的模型。

    > 已支持的语音转文字模型供应商：OpenAI

根据技术变化和用户需求，我们将陆续支持更多 LLM 供应商。

### 托管模型试用服务

我们为 Dify 云服务的用户提供了不同模型的试用额度，请在该额度耗尽前设置你自己的模型供应商，否则将会影响应用的正常使用。

* **OpenAI 托管模型试用：** 我们提供 200 次调用次数供你试用体验，可用于 GPT3.5-turbo、GPT3.5-turbo-16k、text-davinci-003 模型。

### 设置默认模型

Dify 在需要模型时，会根据使用场景来选择设置过的默认模型。在 `设置 > 模型供应商` 中设置默认模型。

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

系统默认推理模型(System Reasoning Model)：设置创建应用使用的默认推理模型,以及对话名称生成、下一步问题建议等功能也会使用该默认推理模型。

### 接入模型设置

在 Dify 的 `设置 > 模型供应商` 中设置要接入的模型。

<figure><img src="../../.gitbook/assets/image-20231210143654461 (1).png" alt=""><figcaption></figcaption></figure>

模型供应商分为两种：

1. 自有模型。该类型的模型供应商提供的是自己开发的模型。如 OpenAI，Anthropic 等。
2. 托管模型。该类型的模型供应商提供的是第三方模型。如 Hugging Face，Replicate 等。

在 Dify 中接入不同类型的模型供应商的方式稍有不同。

**接入自有模型的模型供应商**

接入自有模型的供应商后，Dify 会自动接入该供应商下的所有模型。

在 Dify 中设置对应模型供应商的 API key，即可接入该模型供应商。

{% hint style="info" %}
Dify 使用了 [PKCS1\_OAEP](https://pycryptodome.readthedocs.io/en/latest/src/cipher/oaep.html) 来加密存储用户托管的 API 密钥，每个租户均使用了独立的密钥对进行加密，确保你的 API 密钥不被泄漏。
{% endhint %}

**接入托管模型的模型供应商**

托管类型的供应商上面有很多第三方模型。接入模型需要一个个的添加。具体接入方式如下：

* [Hugging Face](../../development/mo-xing-jie-ru/hugging-face.md)
* [Replicate](../../development/mo-xing-jie-ru/replicate.md)
* [Xinference](../../development/mo-xing-jie-ru/xinference.md)
* [OpenLLM](../../development/mo-xing-jie-ru/openllm.md)

### 使用模型

配置完模型后，就可以在应用中使用这些模型了：

<figure><img src="../../.gitbook/assets/image (166).png" alt=""><figcaption></figcaption></figure>
