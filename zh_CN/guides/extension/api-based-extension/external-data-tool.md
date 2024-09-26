# 外部数据工具

在创建 AI 应用时，开发者可以通过 API 扩展的方式实现使用外部工具获取额外数据组装至 Prompt 中作为 LLM 额外信息。具体的实操过程可以参考 [API 扩展](../api-based-extension/ "mention")。

### 前置条件

请先阅读 [.](./ "mention") 完成 API 服务基础能力的开发和接入。

### 扩展点

`app.external_data_tool.query` 应用外部数据工具查询扩展点。

该扩展点将终端用户传入的应用变量内容和对话输入内容（对话型应用固定参数）作为参数，传给 API。

开发者需要实现对应工具的查询逻辑，并返回字符串类型的查询结果。

#### Request Body <a href="#user-content-request-body" id="user-content-request-body"></a>

```
{
    "point": "app.external_data_tool.query", // 扩展点类型，此处固定为 app.external_data_tool.query
    "params": {
        "app_id": string,  // 应用 ID
        "tool_variable": string,  // 外部数据工具变量名称，表示对应变量工具调用来源
        "inputs": {  // 终端用户传入变量值，key 为变量名，value 为变量值
            "var_1": "value_1",
            "var_2": "value_2",
            ...
        },
        "query": string | null  // 终端用户当前对话输入内容，对话型应用固定参数。
    }
}
```

* Example
  * ```
    {
        "point": "app.external_data_tool.query",
        "params": {
            "app_id": "61248ab4-1125-45be-ae32-0ce91334d021",
            "tool_variable": "weather_retrieve",
            "inputs": {
                "location": "London"
            },
            "query": "How's the weather today?"
        }
    }
    ```

#### API 返回 <a href="#usercontentapi-fan-hui" id="usercontentapi-fan-hui"></a>

```
{
    "result": string
}
```

* Example
  * ```
    {
        "result": "City: London\nTemperature: 10°C\nRealFeel®: 8°C\nAir Quality: Poor\nWind Direction: ENE\nWind Speed: 8 km/h\nWind Gusts: 14 km/h\nPrecipitation: Light rain"
    }
    ```

\\
