# 集成 LangSmith

### 什么是 LangSmith

LangSmith 是一个用于构建生产级 LLM 应用程序的平台，它用于开发、协作、测试、部署和监控 LLM 应用程序。

{% hint style="info" %}
LangSmith 官网介绍：[https://www.langchain.com/langsmith](https://www.langchain.com/langsmith)
{% endhint %}

***

### 如何配置 LangSmith

本章节将指引你注册 LangSmith 并将其集成至 Dify 平台内。

#### 1. 注册/登录 [LangSmith](https://www.langchain.com/langsmith)

#### 2. 创建项目

在 LangSmith 内创建项目，登录后在主页点击 **New Project** 创建一个自己的项目，**项目**将用于与 Dify 内的**应用**关联进行数据监测。

<figure><img src="../../../.gitbook/assets/image (3) (1) (1).png" alt=""><figcaption><p>在 LangSmith 内创建项目</p></figcaption></figure>

创建完成之后在 Projects 内可以查看该项目。

<figure><img src="../../../.gitbook/assets/image (7) (1) (1).png" alt=""><figcaption><p>在 LangSmith 内查看已创建项目</p></figcaption></figure>

#### 3. 创建项目凭据

创建项目凭据，在左侧边栏内找到项目设置 **Settings**。

<figure><img src="../../../.gitbook/assets/image (8) (1) (1).png" alt=""><figcaption><p>项目设置</p></figcaption></figure>

点击 **Create API Key**，创建一个项目凭据。

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1).png" alt=""><figcaption><p>创建一个项目 API Key</p></figcaption></figure>

选择 **Personal Access Token** ，用于后续的 API 身份校验。

<figure><img src="../../../.gitbook/assets/image (5) (1) (1).png" alt=""><figcaption><p>创建一个 API Key</p></figcaption></figure>

将创建的 API key 复制保存。

<figure><img src="../../../.gitbook/assets/image (9) (1).png" alt=""><figcaption><p>复制 API Key</p></figcaption></figure>

#### 4. 将 LangSmith 集成至 Dify 平台

在 Dify 应用内配置 LangSmith。打开需要监测的应用，在左侧边菜单内打开**监测**，点击页面内的**配置。**

<figure><img src="../../../.gitbook/assets/image (11) (1).png" alt=""><figcaption><p>配置 LangSmith</p></figcaption></figure>

点击配置后，将在 LangSmith 内创建的 **API Key** 和**项目名**粘贴到配置内并保存。

<figure><img src="../../../.gitbook/assets/image (12).png" alt=""><figcaption><p>配置 LangSmith</p></figcaption></figure>

{% hint style="info" %}
配置项目名需要与 LangSmith 内设置的项目一致，若项目名不一致，数据同步时 LangSmith 会自动创建一个新的项目。
{% endhint %}

成功保存后可以在当前页面查看监测状态。

<figure><img src="../../../.gitbook/assets/image (15).png" alt=""><figcaption><p>查看配置状态</p></figcaption></figure>

### 在 LangSmith 内查看监测数据

配置完成后， Dify 内应用的调试或生产数据可以在 LangSmith 查看监测数据。

<figure><img src="../../../.gitbook/assets/image (17).png" alt=""><figcaption><p>在 Dify 内调试应用</p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (2) (1) (1).png" alt=""><figcaption><p>在 LangSmith 内查看应用数据</p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (18).png" alt=""><figcaption><p>在 LangSmith 内查看应用数据</p></figcaption></figure>

### 监测数据清单

#### Workflow /Chatflow Trace信息

**用于追踪workflow以及chatflow**

| Workflow                                 | LangSmith Chain            |
| ---------------------------------------- | -------------------------- |
| workflow\_app\_log\_id/workflow\_run\_id | id                         |
| user\_session\_id                        | -放入metadata                |
| workflow\_{id}                           | name                       |
| start\_time                              | start\_time                |
| end\_time                                | end\_time                  |
| inputs                                   | inputs                     |
| outputs                                  | outputs                    |
| 模型token消耗相关                              | usage\_metadata            |
| metadata                                 | extra                      |
| error                                    | error                      |
| \[workflow]                              | tags                       |
| "conversation\_id/workflow时无"            | metadata中的conversation\_id |
| conversion\_id                           | parent\_run\_id            |

**Workflow Trace Info**

* workflow\_id - Workflow的唯一标识
* conversation\_id - 对话ID
* workflow\_run\_id - 此次运行的ID
* tenant\_id - 租户ID
* elapsed\_time - 此次运行耗时
* status - 运行状态
* version - Workflow版本
* total\_tokens - 此次运行使用的token总数
* file\_list - 处理的文件列表
* triggered\_from - 触发此次运行的来源
* workflow\_run\_inputs - 此次运行的输入数据
* workflow\_run\_outputs - 此次运行的输出数据
* error - 此次运行中发生的错误
* query - 运行时使用的查询
* workflow\_app\_log\_id - Workflow应用日志ID
* message\_id - 关联的消息ID
* start\_time - 运行开始时间
* end\_time - 运行结束时间
* workflow node executions - workflow节点运行信息
* Metadata
  * workflow\_id - Workflow的唯一标识
  * conversation\_id - 对话ID
  * workflow\_run\_id - 此次运行的ID
  * tenant\_id - 租户ID
  * elapsed\_time - 此次运行耗时
  * status - 运行状态
  * version - Workflow版本
  * total\_tokens - 此次运行使用的token总数
  * file\_list - 处理的文件列表
  * triggered\_from - 触发来源

#### Message Trace信息

**用于追踪llm对话相关**

| Chat                             | LangSmith LLM              |
| -------------------------------- | -------------------------- |
| message\_id                      | id                         |
| user\_session\_id                | -放入metadata                |
| “message\_{id}"                  | name                       |
| start\_time                      | start\_time                |
| end\_time                        | end\_time                  |
| inputs                           | inputs                     |
| outputs                          | outputs                    |
| 模型token消耗相关                      | usage\_metadata            |
| metadata                         | extra                      |
| error                            | error                      |
| \["message", conversation\_mode] | tags                       |
| conversation\_id                 | metadata中的conversation\_id |
| conversion\_id                   | parent\_run\_id            |

**Message Trace Info**

* message\_id - 消息ID
* message\_data - 消息数据
* user\_session\_id - 用户的session\_id
* conversation\_model - 对话模式
* message\_tokens - 消息中的令牌数
* answer\_tokens - 回答中的令牌数
* total\_tokens - 消息和回答中的总令牌数
* error - 错误信息
* inputs - 输入数据
* outputs - 输出数据
* file\_list - 处理的文件列表
* start\_time - 开始时间
* end\_time - 结束时间
* message\_file\_data - 消息关联的文件数据
* conversation\_mode - 对话模式
* Metadata
  * conversation\_id - 消息所属对话的ID
  * ls\_provider - 模型提供者
  * ls\_model\_name - 模型ID
  * status - 消息状态
  * from\_end\_user\_id - 发送用户的ID
  * from\_account\_id - 发送账户的ID
  * agent\_based - 是否基于代理
  * workflow\_run\_id - 工作流运行ID
  * from\_source - 消息来源
  * message\_id - 消息ID

#### Moderation Trace信息

**用于追踪对话审查**

| Moderation    | LangSmith Tool  |
| ------------- | --------------- |
| user\_id      | -放入metadata     |
| “moderation"  | name            |
| start\_time   | start\_time     |
| end\_time     | end\_time       |
| inputs        | inputs          |
| outputs       | outputs         |
| metadata      | extra           |
| \[moderation] | tags            |
| message\_id   | parent\_run\_id |

**Message Trace Info**

* message\_id - 消息ID
* user\_id: 用户id
* workflow\_app\_log\_id workflow\_app\_log\_id
* inputs - 审查的输入数据
* message\_data - 消息数据
* flagged - 是否被标记为需要注意的内容
* action - 执行的具体行动
* preset\_response - 预设响应
* start\_time - 审查开始时间
* end\_time - 审查结束时间
* Metadata
  * message\_id - 消息ID
  * action - 执行的具体行动
  * preset\_response - 预设响应

#### Suggested Question Trace信息

**用于追踪建议问题**

| Suggested Question     | LangSmith LLM   |
| ---------------------- | --------------- |
| user\_id               | -放入metadata     |
| suggested\_question    | name            |
| start\_time            | start\_time     |
| end\_time              | end\_time       |
| inputs                 | inputs          |
| outputs                | outputs         |
| metadata               | extra           |
| \[suggested\_question] | tags            |
| message\_id            | parent\_run\_id |

**Message Trace Info**

* message\_id - 消息ID
* message\_data - 消息数据
* inputs - 输入的内容
* outputs - 输出的内容
* start\_time - 开始时间
* end\_time - 结束时间
* total\_tokens - 令牌数量
* status - 消息状态
* error - 错误信息
* from\_account\_id - 发送账户的ID
* agent\_based - 是否基于代理
* from\_source - 消息来源
* model\_provider - 模型提供者
* model\_id - 模型ID
* suggested\_question - 建议的问题
* level - 状态级别
* status\_message - 状态信息
* Metadata
  * message\_id - 消息ID
  * ls\_provider - 模型提供者
  * ls\_model\_name - 模型ID
  * status - 消息状态
  * from\_end\_user\_id - 发送用户的ID
  * from\_account\_id - 发送账户的ID
  * workflow\_run\_id - 工作流运行ID
  * from\_source - 消息来源

#### Dataset Retrieval Trace信息

**用于追踪知识库检索**

| Dataset Retrieval     | LangSmith Retriever |
| --------------------- | ------------------- |
| user\_id              | -放入metadata         |
| dataset\_retrieval    | name                |
| start\_time           | start\_time         |
| end\_time             | end\_time           |
| inputs                | inputs              |
| outputs               | outputs             |
| metadata              | extra               |
| \[dataset\_retrieval] | tags                |
| message\_id           | parent\_run\_id     |

**Dataset Retrieval Trace Info**

* message\_id - 消息ID
* inputs - 输入内容
* documents - 文档数据
* start\_time - 开始时间
* end\_time - 结束时间
* message\_data - 消息数据
* Metadata
  * message\_id消息ID
  * ls\_provider模型提供者
  * ls\_model\_name模型ID
  * status消息状态
  * from\_end\_user\_id发送用户的ID
  * from\_account\_id发送账户的ID
  * agent\_based是否基于代理
  * workflow\_run\_id工作流运行ID
  * from\_source消息来源

#### Tool Trace信息

**用于追踪工具调用**

| Tool                  | LangSmith Tool  |
| --------------------- | --------------- |
| user\_id              | -放入metadata     |
| tool\_name            | name            |
| start\_time           | start\_time     |
| end\_time             | end\_time       |
| inputs                | inputs          |
| outputs               | outputs         |
| metadata              | extra           |
| \["tool", tool\_name] | tags            |
| message\_id           | parent\_run\_id |

**Tool Trace Info**

* message\_id消息ID
* tool\_name工具名称
* start\_time开始时间
* end\_time结束时间
* tool\_inputs工具输入
* tool\_outputs工具输出
* message\_data消息数据
* error错误信息，如果存在
* inputs消息的输入内容
* outputs消息的回答内容
* tool\_config工具配置
* time\_cost时间成本
* tool\_parameters工具参数
* file\_url关联文件的URL
* Metadata
  * message\_id消息ID
  * tool\_name工具名称
  * tool\_inputs工具输入
  * tool\_outputs工具输出
  * tool\_config工具配置
  * time\_cost时间成本
  * error错误信息
  * tool\_parameters工具参数
  * message\_file\_id消息文件ID
  * created\_by\_role创建者角色
  * created\_user\_id创建者用户ID

#### Generate Name Trace信息

**用于追踪会话标题生成**

| Generate Name     | LangSmith Tool |
| ----------------- | -------------- |
| user\_id          | -放入metadata    |
| generate\_name    | name           |
| start\_time       | start\_time    |
| end\_time         | end\_time      |
| inputs            | inputs         |
| outputs           | outputs        |
| metadata          | extra          |
| \[generate\_name] | tags           |

**Generate Name Trace Info**

* conversation\_id对话ID
* inputs输入数据
* outputs生成的会话名称
* start\_time开始时间
* end\_time结束时间
* tenant\_id租户ID
* Metadata
  * conversation\_id对话ID
  * tenant\_id租户ID
