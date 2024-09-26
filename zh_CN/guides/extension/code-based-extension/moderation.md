# 敏感内容审查

除了系统内置的内容审查类型，Dify 也支持用户扩展自定义的内容审查规则，该方法适用于私有部署的开发者定制开发。比如企业内部客服，规定用户在查询的时候以及客服回复的时候，除了不可以输入暴力，性和非法行为等相关词语，也不能出现企业自己规定的禁词或违反内部制定的审查逻辑，那么开发者可以在私有部署的 Dify 代码层扩展自定义内容审查规则。

## 快速开始

这里以一个 `Cloud Service` 内容审查扩展为例，步骤如下：

1. 初始化目录
2. 添加前端组件定义文件
3. 添加实现类
4. 预览前端界面
5. 调试扩展

### 1. 初始化目录

新增自定义类型 `Cloud Service`，需要在 `api/core/moderation` 目录下新建相关的目录和文件。

```Plain
.
└── api
    └── core
        └── moderation
            └── cloud_service
                ├── __init__.py
                ├── cloud_service.py
                └── schema.json
```

### 2.添加前端组件规范

* `schema.json`，这里定义了前端组件规范，详细见 [Broken link](broken-reference "mention") 。

```json
{
    "label": {
        "en-US": "Cloud Service",
        "zh-Hans": "云服务"
    },
    "form_schema": [
        {
            "type": "select",
            "label": {
                "en-US": "Cloud Provider",
                "zh-Hans": "云厂商"
            },
            "variable": "cloud_provider",
            "required": true,
            "options": [
                {
                    "label": {
                        "en-US": "AWS",
                        "zh-Hans": "亚马逊"
                    },
                    "value": "AWS"
                },
                {
                    "label": {
                        "en-US": "Google Cloud",
                        "zh-Hans": "谷歌云"
                    },
                    "value": "GoogleCloud"
                },
                {
                    "label": {
                        "en-US": "Azure Cloud",
                        "zh-Hans": "微软云"
                    },
                    "value": "Azure"
                }
            ],
            "default": "GoogleCloud",
            "placeholder": ""
        },
        {
            "type": "text-input",
            "label": {
                "en-US": "API Endpoint",
                "zh-Hans": "API Endpoint"
            },
            "variable": "api_endpoint",
            "required": true,
            "max_length": 100,
            "default": "",
            "placeholder": "https://api.example.com"
        },
        {
            "type": "paragraph",
            "label": {
                "en-US": "API Key",
                "zh-Hans": "API Key"
            },
            "variable": "api_keys",
            "required": true,
            "default": "",
            "placeholder": "Paste your API key here"
        }
    ]
}
```

### 3. 添加实现类

`cloud_service.py` 代码模版，你可以在这里实现具体的业务逻辑。

{% hint style="warning" %}
注意：类变量 name 为自定义类型名称，需要跟目录和文件名保持一致，而且唯一。
{% endhint %}

```python
from core.moderation.base import Moderation, ModerationAction, ModerationInputsResult, ModerationOutputsResult

class CloudServiceModeration(Moderation):
    """
    The name of custom type must be unique, keep the same with directory and file name.
    """
    name: str = "cloud_service"

    @classmethod
    def validate_config(cls, tenant_id: str, config: dict) -> None:
        """
        schema.json validation. It will be called when user save the config.

        Example:
            .. code-block:: python
                config = {
                    "cloud_provider": "GoogleCloud",
                    "api_endpoint": "https://api.example.com",
                    "api_keys": "123456",
                    "inputs_config": {
                        "enabled": True,
                        "preset_response": "Your content violates our usage policy. Please revise and try again."
                    },
                    "outputs_config": {
                        "enabled": True,
                        "preset_response": "Your content violates our usage policy. Please revise and try again."
                    }
                }

        :param tenant_id: the id of workspace
        :param config: the variables of form config
        :return:
        """

        cls._validate_inputs_and_outputs_config(config, True)

        if not config.get("cloud_provider"):
            raise ValueError("cloud_provider is required")

        if not config.get("api_endpoint"):
            raise ValueError("api_endpoint is required")

        if not config.get("api_keys"):
            raise ValueError("api_keys is required")

    def moderation_for_inputs(self, inputs: dict, query: str = "") -> ModerationInputsResult:
        """
        Moderation for inputs.

        :param inputs: user inputs
        :param query: the query of chat app, there is empty if is completion app
        :return: the moderation result
        """
        flagged = False
        preset_response = ""

        if self.config['inputs_config']['enabled']:
            preset_response = self.config['inputs_config']['preset_response']

            if query:
                inputs['query__'] = query
            flagged = self._is_violated(inputs)

        # return ModerationInputsResult(flagged=flagged, action=ModerationAction.overridden, inputs=inputs, query=query)
        return ModerationInputsResult(flagged=flagged, action=ModerationAction.DIRECT_OUTPUT, preset_response=preset_response)

    def moderation_for_outputs(self, text: str) -> ModerationOutputsResult:
        """
        Moderation for outputs.

        :param text: the text of LLM response
        :return: the moderation result
        """
        flagged = False
        preset_response = ""

        if self.config['outputs_config']['enabled']:
            preset_response = self.config['outputs_config']['preset_response']

            flagged = self._is_violated({'text': text})

        # return ModerationOutputsResult(flagged=flagged, action=ModerationAction.overridden, text=text)
        return ModerationOutputsResult(flagged=flagged, action=ModerationAction.DIRECT_OUTPUT, preset_response=preset_response)

    def _is_violated(self, inputs: dict):
        """
        The main logic of moderation.

        :param inputs:
        :return: the moderation result
        """
        return False
```

### 4. 调试扩展

至此，即可在 Dify 应用编排界面选择自定义的 `Cloud Service` 内容审查扩展类型进行调试。\\

## 实现类模版

```python
from core.moderation.base import Moderation, ModerationAction, ModerationInputsResult, ModerationOutputsResult

class CloudServiceModeration(Moderation):
    """
    The name of custom type must be unique, keep the same with directory and file name.
    """
    name: str = "cloud_service"

    @classmethod
    def validate_config(cls, tenant_id: str, config: dict) -> None:
        """
        schema.json validation. It will be called when user save the config.
        
        :param tenant_id: the id of workspace
        :param config: the variables of form config
        :return:
        """
        cls._validate_inputs_and_outputs_config(config, True)
        
        # implement your own logic here

    def moderation_for_inputs(self, inputs: dict, query: str = "") -> ModerationInputsResult:
        """
        Moderation for inputs.

        :param inputs: user inputs
        :param query: the query of chat app, there is empty if is completion app
        :return: the moderation result
        """
        flagged = False
        preset_response = ""
        
        # implement your own logic here
        
        # return ModerationInputsResult(flagged=flagged, action=ModerationAction.overridden, inputs=inputs, query=query)
        return ModerationInputsResult(flagged=flagged, action=ModerationAction.DIRECT_OUTPUT, preset_response=preset_response)

    def moderation_for_outputs(self, text: str) -> ModerationOutputsResult:
        """
        Moderation for outputs.

        :param text: the text of LLM response
        :return: the moderation result
        """
        flagged = False
        preset_response = ""
        
        # implement your own logic here

        # return ModerationOutputsResult(flagged=flagged, action=ModerationAction.overridden, text=text)
        return ModerationOutputsResult(flagged=flagged, action=ModerationAction.DIRECT_OUTPUT, preset_response=preset_response)
```

## 实现类开发详细介绍

### def validate\_config

`schema.json` 表单校验方法，当用户点击「发布」保存配置时调用

* `config` 表单参数
  * `{{variable}}` 表单自定义变量
  * `inputs_config` 输入审查预设回复
    * `enabled` 是否开启
    * `preset_response` 输入预设回复
  * `outputs_config`输出审查预设回复
    * `enabled` 是否开启
    * `preset_response` 输出预设回复

### def moderation\_for\_inputs

输入校验函数

* `inputs` ：终端用户传入变量值
* `query` ：终端用户当前对话输入内容，对话型应用固定参数。
* `ModerationInputsResult`
  * `flagged` 是否违反校验规则
  * `action` 执行动作
    * `direct_output` 直接输出预设回复
    * `overridden` 覆写传入变量值
  * `preset_response` 预设回复（仅当 action=direct\_output 返回）
  * `inputs` 终端用户传入变量值，key 为变量名，value 为变量值（仅当 action=overridden 返回）
  * `query` 覆写的终端用户当前对话输入内容，对话型应用固定参数。（仅当 action=overridden 返回）

### def moderation\_for\_outputs

输出校验函数

* `text` ：模型输出内容
* `moderation_for_outputs` ：输出校验函数
  * `text` ：LLM 回答内容。当 LLM 输出为流式时，此处为 100 字为一个分段的内容。
  * `ModerationOutputsResult`
    * `flagged` 是否违反校验规则
    * `action` 执行动作
      * `direct_output`直接输出预设回复
      * `overridden`覆写传入变量值
    * `preset_response` 预设回复（仅当 action=direct\_output 返回）
    * `text` 覆写的 LLM 回答内容（仅当 action=overridden 返回）。

\\
