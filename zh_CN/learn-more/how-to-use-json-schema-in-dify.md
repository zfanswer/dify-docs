# 如何在 Dify 中使用 JSON Schema 输出功能？

JSON Schema 是一种用于描述 JSON 数据结构的规范，开发者可以通过定义 JSON Schema 结构，指定 LLM 输出严格遵循定义内的数据或内容，例如生成清晰的文档或代码结构。

## 支持 JSON Schema 功能的模型

- `gpt-4o-mini-2024-07-18` and later
- `gpt-4o-2024-08-06` and later

> 如需了解更多关于 OpenAI 系列模型的结构化输出能力，请参考 [Structured Outputs](https://platform.openai.com/docs/guides/structured-outputs/introduction)。

## Structured-outputs 用法

1. 将 LLM 连接到系统中的工具、函数、数据等；在函数定义中设置 `strict: true`，当打开它时，结构化输出（Structured-outputs）功能可确保 LLM 为函数调用生成的参数与你在函数定义中提供的 JSON 架构完全匹配。

2. LLM 回答用户时，按照 JSON Schema 中的定义，以结构化内容格式输出。

## 在 Dify 中开启 JSON Schema

将应用中的 LLM 切换至上述支持 JSON Schema 输出的模型，然后在设置表单开启 `JSON Schema` 并填写 JSON Schema 模板；同时开启 `response_format` 栏并切换至 `json_schema` 格式。

![](../../../img/learn-more-json-schema.png)

LLM 生成的内容支持以下格式输出：

- **Text:** 以文本格式输出

## 定义 JSON Schema 模板

你可以参考以下 JSON Schema 格式并定义模板内容：

```json
{
    "name": "template_schema",
    "description": "A generic template for JSON Schema",
    "strict": true,
    "schema": {
        "type": "object",
        "properties": {
            "field1": {
                "type": "string",
                "description": "Description of field1"
            },
            "field2": {
                "type": "number",
                "description": "Description of field2"
            },
            "field3": {
                "type": "array",
                "description": "Description of field3",
                "items": {
                    "type": "string"
                }
            },
            "field4": {
                "type": "object",
                "description": "Description of field4",
                "properties": {
                    "subfield1": {
                        "type": "string",
                        "description": "Description of subfield1"
                    }
                },
                "required": ["subfield1"],
                "additionalProperties": false
            }
        },
        "required": ["field1", "field2", "field3", "field4"],
        "additionalProperties": false
    }
}
```

步骤指导：

1. 定义基本信息：
  - 设置 `name`：为您的 schema 起一个描述性的名称。
  - 添加 `description`：简要说明 schema 的用途。
  - 设置 `strict`: true：确保严格模式。

2. 创建 `schema` 对象：
  - 设置 `type: "object"`：指定根级别为对象类型。
  - 添加 `properties` 对象：用于定义所有字段。

3. 定义字段：
  - 为每个字段创建一个对象，包含 `type` 和 `description`。
  - 常见类型：`string`, `number`, `boolean`, `array`, `object`。
  - 对于数组，使用 `items` 定义元素类型。
  - 对于对象，递归定义 `properties`。

4. 设置约束：
  - 在每个级别添加 `required` 数组，列出所有必需字段。
  - 在每个对象级别设置 `additionalProperties: false`。

5. 特殊字段处理：
  - 使用 `enum` 限制可选值。
  - 使用 `$ref` 实现递归结构。

## 示例

### 1. 推理链（常规）

**JSON Schema 文件示例**

```json
{
    "name": "math_reasoning",
    "description": "Records steps and final answer for mathematical reasoning",
    "strict": true,
    "schema": {
        "type": "object",
        "properties": {
            "steps": {
                "type": "array",
                "description": "Array of reasoning steps",
                "items": {
                    "type": "object",
                    "properties": {
                        "explanation": {
                            "type": "string",
                            "description": "Explanation of the reasoning step"
                        },
                        "output": {
                            "type": "string",
                            "description": "Output of the reasoning step"
                        }
                    },
                    "required": ["explanation", "output"],
                    "additionalProperties": false
                }
            },
            "final_answer": {
                "type": "string",
                "description": "The final answer to the mathematical problem"
            }
        },
        "additionalProperties": false,
        "required": ["steps", "final_answer"]
    }
}
```

**提示词参考**

```text
You are a helpful math tutor. You will be provided with a math problem,
and your goal will be to output a step by step solution, along with a final answer.
For each step, just provide the output as an equation use the explanation field to detail the reasoning.
```

### UI 生成器（根递归模式）

```json
{
        "name": "ui",
        "description": "Dynamically generated UI",
        "strict": true,
        "schema": {
            "type": "object",
            "properties": {
                "type": {
                    "type": "string",
                    "description": "The type of the UI component",
                    "enum": ["div", "button", "header", "section", "field", "form"]
                },
                "label": {
                    "type": "string",
                    "description": "The label of the UI component, used for buttons or form fields"
                },
                "children": {
                    "type": "array",
                    "description": "Nested UI components",
                    "items": {
                        "$ref": "#"
                    }
                },
                "attributes": {
                    "type": "array",
                    "description": "Arbitrary attributes for the UI component, suitable for any element",
                    "items": {
                        "type": "object",
                        "properties": {
                            "name": {
                                "type": "string",
                                "description": "The name of the attribute, for example onClick or className"
                            },
                            "value": {
                                "type": "string",
                                "description": "The value of the attribute"
                            }
                        },
                      "additionalProperties": false,
                      "required": ["name", "value"]
                    }
                }
            },
            "required": ["type", "label", "children", "attributes"],
            "additionalProperties": false
        }
    }
```

**提示词参考：**

```text
You are a UI generator AI. Convert the user input into a UI.
```

**效果示例：**

![](../../img/best-practice-json-schema-ui-example.png)

## 提示

- 请确保应用提示词内包含如何处理用户输入无法产生有效响应的情况说明。

- 模型将始终尝试遵循提供的模式，如果输入的内容与指定的模式完全无关，则可能会导致 LLM 产生幻觉。

- 如果 LLM 检测到输入与任务不兼容，你可以在提示中包含语言，以指定返回空参数或特定句子。

- 所有字段必须为 `required`，详情请参考[此处](https://platform.openai.com/docs/guides/structured-outputs/supported-schemas)。

- [additionalProperties：false](https://platform.openai.com/docs/guides/structured-outputs/additionalproperties-false-must-always-be-set-in-objects) 必须始终在对象中设置

- 模式的根级别对象必须是一个对象

## 附录

- [Introduction to Structured Outputs](https://cookbook.openai.com/examples/structured_outputs_intro)

- [Structured Output](https://platform.openai.com/docs/guides/structured-outputs/json-mode?context=without_parse)
