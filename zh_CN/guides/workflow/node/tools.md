# 工具

“工具”节点可以为工作流提供强大的第三方能力支持，分为以下三种类型：

* **内置工具**，Dify 第一方提供的工具，使用该工具前可能需要先给工具进行 **授权**。
* **自定义工具**，通过 [OpenAPI/Swagger 标准格式](https://swagger.io/specification/)导入或配置的工具。如果内置工具无法满足使用需求，你可以在 **Dify 菜单导航 --工具** 内创建自定义工具。
* **工作流**，你可以编排一个更复杂的工作流，并将其发布为工具。详细说明请参考[工具配置说明](https://docs.dify.ai/v/zh-hans/guides/tools)。

### 添加工具节点

添加节点时，选择右侧的 “工具” tab 页。配置工具节点一般分为两个步骤：

1. 对工具授权/创建自定义工具/将工作流发布为工具
2. 配置工具输入和参数

<figure><img src="../../../.gitbook/assets/image (231).png" alt="" width="258"><figcaption><p>工具选择</p></figcaption></figure>

工具节点可以连接其它节点，通过[变量](https://docs.dify.ai/v/zh-hans/guides/workflow/variables)处理和传递数据。

<figure><img src="../../../.gitbook/assets/image (232).png" alt=""><figcaption><p>配置 Google 搜索工具检索外部知识</p></figcaption></figure>

### 将工作流应用发布为工具

工作流应用可以被发布为工具，并被其它工作流内的节点所应用。关于如何创建自定义工具和配置工具，请参考[工具配置说明](https://docs.dify.ai/v/zh-hans/guides/tools)。
