---
description: 对于已经熟悉 LLM 应用技术栈的技术人士来说，这份文档将是您了解 Dify 独特优势的捷径。让您能够明智地比较和选择，甚至向同事和朋友推荐。
---

# 特性与技术规格

{% hint style="info" %}
在 Dify，我们采用透明化的产品特性和技术规格政策，确保您在全面了解我们产品的基础上做出决策。这种透明度不仅有利于您的技术选型，也促进了社区成员对产品的深入理解和积极贡献。
{% endhint %}

### 项目基础信息

<table data-header-hidden data-full-width="false"><thead><tr><th width="156"></th><th></th></tr></thead><tbody><tr><td>项目设立</td><td>2023 年 3 月</td></tr><tr><td>开源协议</td><td><a href="../../policies/open-source.md">基于 Apache License 2.0 有限商业许可</a></td></tr><tr><td>官方研发团队</td><td>超过 15 名全职员工</td></tr><tr><td>社区贡献者</td><td><a href="https://ossinsight.io/analyze/langgenius/dify">超过 290 人</a> （截止 2024 Q2）</td></tr><tr><td>后端技术</td><td>Python/Flask/PostgreSQL</td></tr><tr><td>前端技术</td><td>Next.js</td></tr><tr><td>代码行数</td><td>超过 13 万行</td></tr><tr><td>发版周期</td><td>平均每周一次</td></tr></tbody></table>

### 技术特性

<table data-header-hidden><thead><tr><th width="258"></th><th></th></tr></thead><tbody><tr><td>LLM 推理引擎</td><td>Dify Runtime ( 自 v0.4 起移除了 LangChain)</td></tr><tr><td>商业模型支持</td><td><p><strong>10+ 家</strong>，包括 OpenAI 与 Anthropic</p><p>新的主流模型通常在 48 小时内完成接入</p></td></tr><tr><td>MaaS 供应商支持</td><td><strong>7 家</strong>，Hugging Face，Replicate，AWS Bedrock，NVIDIA，GroqCloud，together.ai，OpenRouter</td></tr><tr><td>本地模型推理 Runtime 支持</td><td>6 <strong>家</strong>，Xoribits（推荐），OpenLLM，LocalAI，ChatGLM，Ollama，NVIDIA TIS </td></tr><tr><td>OpenAI 接口标准模型接入支持</td><td><strong>∞ 家</strong></td></tr><tr><td>多模态技术</td><td><p>ASR 模型</p><p>GPT-4o 规格的富文本模型</p></td></tr><tr><td>预置应用类型</td><td><p>对话型应用</p><p>文本生成应用<br>Agent<br>工作流</p></td></tr><tr><td>Prompt 即服务编排</td><td><p>广受好评的可视化的 Prompt 编排界面，在同一个界面中修改 Prompt 并预览效果<br></p><p><strong>编排模式</strong></p><ul><li>简易模式编排</li><li>Assistant 模式编排</li><li>Flow 模式编排</li></ul><p><strong>Prompt 变量类型</strong></p><ul><li>字符串</li><li>单选枚举</li><li>外部 API</li><li>文件（Q3 即将推出）</li></ul></td></tr><tr><td>Agentic Workflow 特性</td><td><p>行业领先的可视化流程编排界面，所见即所得的节点调试，可插拔的 DSL，原生的代码运行时，构建更复杂、可靠、稳定的 LLM 应用。</p><p></p><p><strong>支持节点</strong></p><ul><li>LLM</li><li>知识库检索</li><li>问题分类</li><li>条件分支</li><li>代码执行</li><li>模板转换</li><li>HTTP 请求</li><li>工具</li></ul></td></tr><tr><td>RAG 特性</td><td><p>首创的可视化的知识库管理界面，支持分段预览和召回效果测试。<br><br><strong>索引方式</strong></p><ul><li>关键词</li><li>文本向量</li><li>由 LLM 辅助的问题-分段模式</li></ul><p><strong>检索方式</strong></p><ul><li>关键词</li><li>文本相似度匹配</li><li>混合检索</li><li>N 选 1 模式（即将下线）</li><li>多路召回</li></ul><p><strong>召回优化技术</strong></p><ul><li>使用 ReRank 模型</li></ul></td></tr><tr><td>ETL 技术</td><td><p>支持对 TXT、Markdown、PDF、HTML、DOC、CSV 等格式文件进行自动清洗，内置的 Unstructured 服务开启后可获得最大化支持。</p><p>支持同步来自 Notion 的文档为知识库。<br>支持同步网页为知识库。</p></td></tr><tr><td>向量数据库支持</td><td>Qdrant（推荐），Weaviate，Zilliz/Milvus，Pgvector，Pgvector-rs，Chroma，OpenSearch，TiDB，Tencent Vector，Oracle，Relyt</td></tr><tr><td>Agent 技术</td><td><p>ReAct，Function Call<br></p><p><strong>工具支持</strong></p><ul><li>可调用 OpenAI Plugin 标准的工具</li><li>可直接加载 OpenAPI Specification 的 API 作为工具</li></ul><p><strong>内置工具</strong></p><ul><li>40+ 款（截止 2024 Q2）</li></ul></td></tr><tr><td>日志</td><td>支持，可基于日志进行标注</td></tr><tr><td>标注回复</td><td>基于经人类标注的 Q&#x26;A 对，可用于相似度对比回复<br>可导出为供模型微调环节使用的数据格式</td></tr><tr><td>内容审查机制</td><td>OpenAI Moderation 或外部 API</td></tr><tr><td>团队协同</td><td>工作空间与多成员管理支持</td></tr><tr><td>API 规格</td><td>RESTful，已覆盖大部分功能</td></tr><tr><td>部署方式</td><td>Docker，Helm</td></tr></tbody></table>

