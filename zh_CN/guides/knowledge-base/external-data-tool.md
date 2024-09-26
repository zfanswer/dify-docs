# 外部数据工具

## 功能介绍

此前 [.](./ "mention") 功能允许开发者可以直接上传各类格式的长文本、结构化数据来构建数据集，使 AI 应用基于用户上传的最新上下文进行对话。

而本次更新的**外部数据工具**赋能开发者可以使用自有的搜索能力或内部知识库等外部数据作为 LLM 的上下文，通过 API 扩展的方式实现外部数据的获取并嵌入提示词。相比在云端上传数据集，使用**外部数据工具**可以在保障私有数据安全，自定义搜索，获取实时数据等方面有显著优势。

## 具体实现

当终端用户向对话系统提出请求时，平台后端会触发外部数据工具（即调用自己的 API），它会查询用户问题相关的外部信息，如员工资料、实时记录等，通过 API 返回与当前请求相关的部分。平台后端会将返回的结果组装成文本作为上下文注入到提示词中，以输出更加个性化和符合用户需求的回复内容。

## 操作说明

1. 在使用外部数据工具之前，你需要准备一个 API 和用于鉴权的 API Key，请阅读[external-data-tool.md](../extension/api-based-extension/external-data-tool.md "mention")
2. Dify 提供了集中式的 API 管理，在设置界面统一添加 API 扩展配置后，即可在 Dify 上的各类应用中直接使用。

<figure><img src="../../.gitbook/assets/api_based.png" alt=""><figcaption><p>API-based Extension</p></figcaption></figure>

3. 我们以“查询天气”为例，在“新增基于 API 的扩展”对话框输入名字，API 端点，API Key。保存后我们就可以调用 API 了。

<figure><img src="../../.gitbook/assets/weather inquiry.png" alt=""><figcaption><p>Weather Inquiry</p></figcaption></figure>

4. 在提示词编排页面，点击“工具”右侧的“+添加”按钮，在打开的“添加 工具”对话框，填写名称和变量名称（变量名称会被引用到提示词中，请填写英文），以及选择第 2 步中已经添加的基于 API 的扩展。

<figure><img src="../../.gitbook/assets/api_based_extension1.png" alt=""><figcaption><p>External_data_tool</p></figcaption></figure>

5. 这样，我们在提示词编排框就可以把查询到的外部数据拼装到提示词中。比如我们要查询今天的伦敦天气，可以添加`location` 变量，输入"London"，结合外部数据工具的扩展变量名称`weather_data`，调试输出如下：

<figure><img src="../../.gitbook/assets/Weather_search_tool.jpeg" alt=""><figcaption><p>Weather_search_tool</p></figcaption></figure>

在对话日志中，我们也可以看到 API 返回的实时数据：

<figure><img src="../../.gitbook/assets/log.jpeg" alt="" width="335"><figcaption><p>Prompt Log</p></figcaption></figure>
