# 快速接入工具

这里我们以 GoogleSearch 为例，介绍如何快速接入一个工具。

### 1. 准备工具供应商 yaml

#### 介绍

这个 yaml 将包含工具供应商的信息，包括供应商名称、图标、作者等详细信息，以帮助前端灵活展示。

#### 示例

我们需要在 `core/tools/provider/builtin`下创建一个`google`模块（文件夹），并创建`google.yaml`，名称必须与模块名称一致。

后续，我们关于这个工具的所有操作都将在这个模块下进行。

```yaml
identity: # 工具供应商的基本信息
  author: Dify # 作者
  name: google # 名称，唯一，不允许和其他供应商重名
  label: # 标签，用于前端展示
    en_US: Google # 英文标签
    zh_Hans: Google # 中文标签
  description: # 描述，用于前端展示
    en_US: Google # 英文描述
    zh_Hans: Google # 中文描述
  icon: icon.svg # 图标，需要放置在当前模块的_assets文件夹下

```

* `identity` 字段是必须的，它包含了工具供应商的基本信息，包括作者、名称、标签、描述、图标等
  *   图标需要放置在当前模块的`_assets`文件夹下，可以参考这里：api/core/tools/provider/builtin/google/\_assets/icon.svg

      ```xml
      <svg xmlns="http://www.w3.org/2000/svg" width="24" height="25" viewBox="0 0 24 25" fill="none">
        <path d="M22.501 12.7332C22.501 11.8699 22.4296 11.2399 22.2748 10.5865H12.2153V14.4832H18.12C18.001 15.4515 17.3582 16.9099 15.9296 17.8898L15.9096 18.0203L19.0902 20.435L19.3106 20.4565C21.3343 18.6249 22.501 15.9298 22.501 12.7332Z" fill="#4285F4"/>
        <path d="M12.214 23C15.1068 23 17.5353 22.0666 19.3092 20.4567L15.9282 17.8899C15.0235 18.5083 13.8092 18.9399 12.214 18.9399C9.38069 18.9399 6.97596 17.1083 6.11874 14.5766L5.99309 14.5871L2.68583 17.0954L2.64258 17.2132C4.40446 20.6433 8.0235 23 12.214 23Z" fill="#34A853"/>
        <path d="M6.12046 14.5766C5.89428 13.9233 5.76337 13.2233 5.76337 12.5C5.76337 11.7766 5.89428 11.0766 6.10856 10.4233L6.10257 10.2841L2.75386 7.7355L2.64429 7.78658C1.91814 9.20993 1.50146 10.8083 1.50146 12.5C1.50146 14.1916 1.91814 15.7899 2.64429 17.2132L6.12046 14.5766Z" fill="#FBBC05"/>
        <path d="M12.2141 6.05997C14.2259 6.05997 15.583 6.91163 16.3569 7.62335L19.3807 4.73C17.5236 3.03834 15.1069 2 12.2141 2C8.02353 2 4.40447 4.35665 2.64258 7.78662L6.10686 10.4233C6.97598 7.89166 9.38073 6.05997 12.2141 6.05997Z" fill="#EB4335"/>
      </svg>
      ```

### 2. 准备供应商凭据

Google 作为一个第三方工具，使用了 SerpApi 提供的 API，而 SerpApi 需要一个 API Key 才能使用，那么就意味着这个工具需要一个凭据才可以使用，而像`wikipedia`这样的工具，就不需要填写凭据字段，可以参考这里：api/core/tools/provider/builtin/wikipedia/wikipedia.yaml

```yaml
identity:
  author: Dify
  name: wikipedia
  label:
    en_US: Wikipedia
    zh_Hans: 维基百科
    pt_BR: Wikipedia
  description:
    en_US: Wikipedia is a free online encyclopedia, created and edited by volunteers around the world.
    zh_Hans: 维基百科是一个由全世界的志愿者创建和编辑的免费在线百科全书。
    pt_BR: Wikipedia is a free online encyclopedia, created and edited by volunteers around the world.
  icon: icon.svg
credentials_for_provider:
```

配置好凭据字段后效果如下：

```yaml
identity:
  author: Dify
  name: google
  label:
    en_US: Google
    zh_Hans: Google
  description:
    en_US: Google
    zh_Hans: Google
  icon: icon.svg
credentials_for_provider: # 凭据字段
  serpapi_api_key: # 凭据字段名称
    type: secret-input # 凭据字段类型
    required: true # 是否必填
    label: # 凭据字段标签
      en_US: SerpApi API key # 英文标签
      zh_Hans: SerpApi API key # 中文标签
    placeholder: # 凭据字段占位符
      en_US: Please input your SerpApi API key # 英文占位符
      zh_Hans: 请输入你的 SerpApi API key # 中文占位符
    help: # 凭据字段帮助文本
      en_US: Get your SerpApi API key from SerpApi # 英文帮助文本
      zh_Hans: 从 SerpApi 获取您的 SerpApi API key # 中文帮助文本
    url: https://serpapi.com/manage-api-key # 凭据字段帮助链接

```

* `type`：凭据字段类型，目前支持`secret-input`、`text-input`、`select` 三种类型，分别对应密码输入框、文本输入框、下拉框，如果为`secret-input`，则会在前端隐藏输入内容，并且后端会对输入内容进行加密。

### 3. 准备工具 yaml

一个供应商底下可以有多个工具，每个工具都需要一个 yaml 文件来描述，这个文件包含了工具的基本信息、参数、输出等。

仍然以 GoogleSearch 为例，我们需要在`google`模块下创建一个`tools`模块，并创建`tools/google_search.yaml`，内容如下。

```yaml
identity: # 工具的基本信息
  name: google_search # 工具名称，唯一，不允许和其他工具重名
  author: Dify # 作者
  label: # 标签，用于前端展示
    en_US: GoogleSearch # 英文标签
    zh_Hans: 谷歌搜索 # 中文标签
description: # 描述，用于前端展示
  human: # 用于前端展示的介绍，支持多语言
    en_US: A tool for performing a Google SERP search and extracting snippets and webpages.Input should be a search query.
    zh_Hans: 一个用于执行 Google SERP 搜索并提取片段和网页的工具。输入应该是一个搜索查询。
  llm: A tool for performing a Google SERP search and extracting snippets and webpages.Input should be a search query. # 传递给 LLM 的介绍，为了使得LLM更好理解这个工具，我们建议在这里写上关于这个工具尽可能详细的信息，让 LLM 能够理解并使用这个工具
parameters: # 参数列表
  - name: query # 参数名称
    type: string # 参数类型
    required: true # 是否必填
    label: # 参数标签
      en_US: Query string # 英文标签
      zh_Hans: 查询语句 # 中文标签
    human_description: # 用于前端展示的介绍，支持多语言
      en_US: used for searching
      zh_Hans: 用于搜索网页内容
    llm_description: key words for searching # 传递给LLM的介绍，同上，为了使得LLM更好理解这个参数，我们建议在这里写上关于这个参数尽可能详细的信息，让LLM能够理解这个参数
    form: llm # 表单类型，llm表示这个参数需要由Agent自行推理出来，前端将不会展示这个参数
  - name: result_type
    type: select # 参数类型
    required: true
    options: # 下拉框选项
      - value: text
        label:
          en_US: text
          zh_Hans: 文本
      - value: link
        label:
          en_US: link
          zh_Hans: 链接
    default: link
    label:
      en_US: Result type
      zh_Hans: 结果类型
    human_description:
      en_US: used for selecting the result type, text or link
      zh_Hans: 用于选择结果类型，使用文本还是链接进行展示
    form: form # 表单类型，form表示这个参数需要由用户在对话开始前在前端填写

```

* `identity` 字段是必须的，它包含了工具的基本信息，包括名称、作者、标签、描述等
* `parameters` 参数列表
  * `name` 参数名称，唯一，不允许和其他参数重名
  * `type` 参数类型，目前支持`string`、`number`、`boolean`、`select` 四种类型，分别对应字符串、数字、布尔值、下拉框
  * `required` 是否必填
    * 在`llm`模式下，如果参数为必填，则会要求 Agent 必须要推理出这个参数
    * 在`form`模式下，如果参数为必填，则会要求用户在对话开始前在前端填写这个参数
  * `options` 参数选项
    * 在`llm`模式下，Dify 会将所有选项传递给 LLM，LLM 可以根据这些选项进行推理
    * 在`form`模式下，`type`为`select`时，前端会展示这些选项
  * `default` 默认值
  * `label` 参数标签，用于前端展示
  * `human_description` 用于前端展示的介绍，支持多语言
  * `llm_description` 传递给 LLM 的介绍，为了使得 LLM 更好理解这个参数，我们建议在这里写上关于这个参数尽可能详细的信息，让 LLM 能够理解这个参数
  * `form` 表单类型，目前支持`llm`、`form`两种类型，分别对应 Agent 自行推理和前端填写

### 4. 准备工具代码

当完成工具的配置以后，我们就可以开始编写工具代码了，主要用于实现工具的逻辑。

在`google/tools`模块下创建`google_search.py`，内容如下。

```python
from core.tools.tool.builtin_tool import BuiltinTool
from core.tools.entities.tool_entities import ToolInvokeMessage

from typing import Any, Dict, List, Union

class GoogleSearchTool(BuiltinTool):
    def _invoke(self, 
                user_id: str,
               tool_Parameters: Dict[str, Any], 
        ) -> Union[ToolInvokeMessage, List[ToolInvokeMessage]]:
        """
            invoke tools
        """
        query = tool_Parameters['query']
        result_type = tool_Parameters['result_type']
        api_key = self.runtime.credentials['serpapi_api_key']
        # TODO: search with serpapi
        result = SerpAPI(api_key).run(query, result_type=result_type)

        if result_type == 'text':
            return self.create_text_message(text=result)
        return self.create_link_message(link=result)
```

#### 参数

工具的整体逻辑都在`_invoke`方法中，这个方法接收两个参数：`user_id`和`tool_Parameters`，分别表示用户 ID 和工具参数

#### 返回数据

在工具返回时，你可以选择返回一个消息或者多个消息，这里我们返回一个消息，使用`create_text_message`和`create_link_message`可以创建一个文本消息或者一个链接消息。

### 5. 准备供应商代码

最后，我们需要在供应商模块下创建一个供应商类，用于实现供应商的凭据验证逻辑，如果凭据验证失败，将会抛出`ToolProviderCredentialValidationError`异常。

在`google`模块下创建`google.py`，内容如下。

```python
from core.tools.entities.tool_entities import ToolInvokeMessage, ToolProviderType
from core.tools.tool.tool import Tool
from core.tools.provider.builtin_tool_provider import BuiltinToolProviderController
from core.tools.errors import ToolProviderCredentialValidationError

from core.tools.provider.builtin.google.tools.google_search import GoogleSearchTool

from typing import Any, Dict

class GoogleProvider(BuiltinToolProviderController):
    def _validate_credentials(self, credentials: Dict[str, Any]) -> None:
        try:
            # 1. 此处需要使用 GoogleSearchTool()实例化一个 GoogleSearchTool，它会自动加载 GoogleSearchTool 的 yaml 配置，但是此时它内部没有凭据信息
            # 2. 随后需要使用 fork_tool_runtime 方法，将当前的凭据信息传递给 GoogleSearchTool
            # 3. 最后 invoke 即可，参数需要根据 GoogleSearchTool 的 yaml 中配置的参数规则进行传递
            GoogleSearchTool().fork_tool_runtime(
                meta={
                    "credentials": credentials,
                }
            ).invoke(
                user_id='',
                tool_Parameters={
                    "query": "test",
                    "result_type": "link"
                },
            )
        except Exception as e:
            raise ToolProviderCredentialValidationError(str(e))
```

### 完成

当上述步骤完成以后，我们就可以在前端看到这个工具了，并且可以在 Agent 中使用这个工具。

当然，因为 google\_search 需要一个凭据，在使用之前，还需要在前端配置它的凭据。

<figure><img src="../../.gitbook/assets/Feb 4, 2024.png" alt=""><figcaption></figcaption></figure>
