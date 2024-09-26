# 外部数据工具

外部数据工具用于在终端用户提交数据后，利用外部工具获取额外数据组装至提示词中作为 LLM 额外上下文信息。Dify 默认提供了外部 API 调用的工具，具体参见 [api-based-extension](../api-based-extension/ "mention")。

而对于本地部署 Dify 的开发者，为了满足更加定制化的需求，或者不希望额外开发一个 API Server，可以直接在 Dify 服务的基础上，以插件的形式插入定制的外部数据工具实现逻辑。扩展自定义工具后，将会在工具类型的下拉列表中增加您的自定义工具选项，团队成员即可使用自定义的工具来获取外部数据。

## 快速开始

这里以一个 `天气查询` 外部数据工具扩展为例，步骤如下：

1. 初始化目录
2. 添加前端表单规范
3. 添加实现类
4. 预览前端界面
5. 调试扩展

### 1. **初始化目录**

新增自定义类型 `Weather Search` ，需要在 `api/core/external_data_tool` 目录下新建相关的目录和文件。

```python
.
└── api
    └── core
        └── external_data_tool
            └── weather_search
                ├── __init__.py
                ├── weather_search.py
                └── schema.json
```

### 2. **添加前端组件规范**

* `schema.json`，这里定义了前端组件规范，详细见 [Broken link](broken-reference "mention")

```json
{
    "label": {
        "en-US": "Weather Search",
        "zh-Hans": "天气查询"
    },
    "form_schema": [
        {
            "type": "select",
            "label": {
                "en-US": "Temperature Unit",
                "zh-Hans": "温度单位"
            },
            "variable": "temperature_unit",
            "required": true,
            "options": [
                {
                    "label": {
                        "en-US": "Fahrenheit",
                        "zh-Hans": "华氏度"
                    },
                    "value": "fahrenheit"
                },
                {
                    "label": {
                        "en-US": "Centigrade",
                        "zh-Hans": "摄氏度"
                    },
                    "value": "centigrade"
                }
            ],
            "default": "centigrade",
            "placeholder": "Please select temperature unit"
        }
    ]
}
```

### 3. 添加实现类

`weather_search.py` 代码模版，你可以在这里实现具体的业务逻辑。

{% hint style="warning" %}
注意：类变量 name 为自定义类型名称，需要跟目录和文件名保持一致，而且唯一。
{% endhint %}

```python
from typing import Optional

from core.external_data_tool.base import ExternalDataTool


class WeatherSearch(ExternalDataTool):
    """
    The name of custom type must be unique, keep the same with directory and file name.
    """
    name: str = "weather_search"

    @classmethod
    def validate_config(cls, tenant_id: str, config: dict) -> None:
        """
        schema.json validation. It will be called when user save the config.

        Example:
            .. code-block:: python
                config = {
                    "temperature_unit": "centigrade"
                }

        :param tenant_id: the id of workspace
        :param config: the variables of form config
        :return:
        """

        if not config.get('temperature_unit'):
            raise ValueError('temperature unit is required')

    def query(self, inputs: dict, query: Optional[str] = None) -> str:
        """
        Query the external data tool.

        :param inputs: user inputs
        :param query: the query of chat app
        :return: the tool query result
        """
        city = inputs.get('city')
        temperature_unit = self.config.get('temperature_unit')

        if temperature_unit == 'fahrenheit':
            return f'Weather in {city} is 32°F'
        else:
            return f'Weather in {city} is 0°C'
```

### 4. **调试扩展**

至此，即可在 Dify 应用编排界面选择自定义的 `Weather Search` 外部数据工具扩展类型进行调试。

## 实现类模版

```python
from typing import Optional

from core.external_data_tool.base import ExternalDataTool


class WeatherSearch(ExternalDataTool):
    """
    The name of custom type must be unique, keep the same with directory and file name.
    """
    name: str = "weather_search"

    @classmethod
    def validate_config(cls, tenant_id: str, config: dict) -> None:
        """
        schema.json validation. It will be called when user save the config.

        :param tenant_id: the id of workspace
        :param config: the variables of form config
        :return:
        """

        # implement your own logic here

    def query(self, inputs: dict, query: Optional[str] = None) -> str:
        """
        Query the external data tool.

        :param inputs: user inputs
        :param query: the query of chat app
        :return: the tool query result
        """
       
        # implement your own logic here
        return "your own data."
```

### 实现类开发详细介绍

### def validate\_config

`schema.json` 表单校验方法，当用户点击「发布」保存配置时调用

* `config` 表单参数
  * `{{variable}}` 表单自定义变量

### def query

用户自定义数据查询实现，返回的结果将会被替换到指定的变量。

* `inputs` ：终端用户传入变量值
* `query` ：终端用户当前对话输入内容，对话型应用固定参数。
