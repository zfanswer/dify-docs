# 工具

### 工具定义

工具可以扩展 LLM 的能力，比如联网搜索、科学计算或绘制图片，赋予并增强了 LLM 连接外部世界的能力。Dify 提供了两种工具类型：**第一方工具**和**自定义工具**。

你可以直接使用 Dify 生态提供的第一方内置工具，或者轻松导入自定义的 API 工具（目前支持 OpenAPI / Swagger 和 OpenAI Plugin 规范）。

#### 工具的作用：

1. 工具使用户可以在 Dify 上创建更强大的 AI 应用，如你可以为智能助理型应用（Agent）编排合适的工具，它可以通过任务推理、步骤拆解、调用工具完成复杂任务。
2. 方便将你的应用与其他系统或服务连接，与外部环境交互，如代码执行、对专属信息源的访问等。

### 如何配置第一方工具

<figure><img src="../../.gitbook/assets/image (131).png" alt=""><figcaption><p>第一方工具列表</p></figcaption></figure>

Dify 目前已支持：

<table><thead><tr><th width="154">工具</th><th>工具描述</th></tr></thead><tbody><tr><td>谷歌搜索</td><td>用于执行 Google SERP 搜索并提取片段和网页的工具。输入应该是一个搜索查询</td></tr><tr><td>维基百科</td><td>用于执行维基百科搜索并提取片段和网页的工具。</td></tr><tr><td>DALL-E 绘画</td><td>用于通过自然语言输入生成高质量图片</td></tr><tr><td>网页抓取</td><td>用于爬取网页数据的工具</td></tr><tr><td>WolframAlpha</td><td>一个强大的计算知识引擎，能根据问题直接给出标准化答案，同时具有强大的数学计算功能</td></tr><tr><td>图表生成</td><td>用于生成可视化图表的工具，你可以通过它来生成柱状图、折线图、饼图等各类图表</td></tr><tr><td>当前时间</td><td>用于查询当前时间的工具</td></tr><tr><td>雅虎财经</td><td>获取并整理出最新的新闻、股票报价等一切你想要的财经信息。</td></tr><tr><td>Stable Diffusion</td><td>一个可以在本地部署的图片生成的工具，您可以使用 stable-diffusion-webui 来部署它</td></tr><tr><td>Vectorizer</td><td>一个将 PNG 和 JPG 图像快速轻松地转换为 SVG 矢量图的工具。</td></tr><tr><td>YouTube</td><td>一个用于获取油管频道视频统计数据的工具</td></tr></tbody></table>

{% hint style="info" %}
欢迎您为 Dify 贡献自己开发的工具，关于如何贡献的具体方法请查看 [Dify 开发贡献文档](https://github.com/langgenius/dify/blob/main/CONTRIBUTING.md)，您的任何支持对我们都是极为宝贵的。
{% endhint %}

#### 第一方工具授权

若你需要直接使用 Dify 生态提供的第一方内置工具，你需要在使用前配置相应的凭据。

<figure><img src="../../.gitbook/assets/image (134).png" alt=""><figcaption><p>配置第一方工具凭据</p></figcaption></figure>

凭据校验成功后工具会显示“已授权”状态。配置凭据后，工作区中的所有成员都可以在编排应用程序时使用此工具。

<figure><img src="../../.gitbook/assets/image (136).png" alt=""><figcaption><p>第一方工具已授权</p></figcaption></figure>

### 如何创建自定义工具

你可以在“工具-自定义工具”内导入自定义的 API 工具，目前支持 OpenAPI / Swagger 和 ChatGPT Plugin 规范。你可以将 OpenAPI schema 内容直接粘贴或从 URL 内导入。关于 OpenAPI / Swagger 规范您可以查看[官方文档说明](https://swagger.io/specification/)。

工具目前支持两种鉴权方式：无鉴权 和 API Key。

<figure><img src="../../.gitbook/assets/image (147).png" alt=""><figcaption><p>创建自定义工具</p></figcaption></figure>

在导入 Schema 内容后系统会主动解析文件内的参数，并可预览工具具体的参数、 方法、路径。您也可以在此对工具参数进行测试。

<figure><img src="../../.gitbook/assets/image (148).png" alt=""><figcaption><p>自定义工具参数测试</p></figcaption></figure>

完成自定义工具创建之后，工作区中的所有成员都可以在“工作室”内编排应用程序时使用此工具。

<figure><img src="../../.gitbook/assets/image (150).png" alt=""><figcaption><p>已添加自定义工具</p></figcaption></figure>

#### Cloudflare Workers

您也可以使用 [dify-tools-worker](https://github.com/crazywoola/dify-tools-worker) 来快速部署自定义工具。该工具提供了：

* 可以导入 Dify 的路由 `https://difytoolsworker.yourname.workers.dev/doc`, 提供了 OpenAPI 兼容的接口文档
* API 的实现代码，可以直接部署到 Cloudflare Workers

### 如何在应用内使用工具

目前，您可以在“工作室”中创建**智能助手型应用**时，将已配置好凭据的工具在其中使用。

<figure><img src="../../.gitbook/assets/image (139).png" alt=""><figcaption><p>创建智能助手型应用时添加工具</p></figcaption></figure>

以下图为例，在财务分析应用内添加工具后，智能助手将在需要时自主调用工具，从工具中查询财务报告数据，并将数据分析后完成与用户之间的对话。

<figure><img src="../../.gitbook/assets/image (144).png" alt=""><figcaption><p>智能助手在对话中完成工具调用回复问题</p></figcaption></figure>
