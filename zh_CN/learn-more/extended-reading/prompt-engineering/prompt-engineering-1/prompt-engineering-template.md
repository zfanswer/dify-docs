# 提示词初始模版参考

为了实现对 LLM 更加定制化的要求来满足开发人员的需要，Dify 在**专家模式**下将内置的完整提示词完全开放，并在编排界面提供了初始模版。以下是四种初始模版参考：

### 1. 使用聊天模型构建对话型应用模版

* **SYSTEM**

```
Use the following context as your learned knowledge, inside <context></context> XML tags.

<context>
{{#context#}}
</context>

When answer to user:
- If you don't know, just say that you don't know.
- If you don't know when you are not sure, ask for clarification.
Avoid mentioning that you obtained the information from the context.
And answer according to the language of the user's question.
{{pre_prompt}}
```

* **USER**

```
{{Query}} //这里输入查询的变量
```

* **ASSISTANT**

```Python
"" 
```

#### **模板结构（Prompt Structure）：**

* 上下文（`Context`）
* 预编排提示词（`Pre-prompt`）
* 查询变量（`Query`）

### 2. 使用聊天模型构建文本生成型应用模版

* **SYSTEM**

```
Use the following context as your learned knowledge, inside <context></context> XML tags.

<context>
{{#context#}}
</context>

When answer to user:
- If you don't know, just say that you don't know.
- If you don't know when you are not sure, ask for clarification.
Avoid mentioning that you obtained the information from the context.
And answer according to the language of the user's question.
{{pre_prompt}}
```

* **USER**

```
{{Query}} //这里输入查询的变量，常用的是输入段落形式的变量
```

* **ASSISTANT**

```Python
"" 
```

#### **模板结构（Prompt Structure）：**

* 上下文（`Context`）
* 预编排提示词（`Pre-prompt`）
* 查询变量（`Query`）

### 3. 使用文本补全模型构建对话型应用模版

```Python
Use the following context as your learned knowledge, inside <context></context> XML tags.

<context>
{{#context#}}
</context>

When answer to user:
- If you don't know, just say that you don't know.
- If you don't know when you are not sure, ask for clarification.
Avoid mentioning that you obtained the information from the context.
And answer according to the language of the user's question.

{{pre_prompt}}

Here is the chat histories between human and assistant, inside <histories></histories> XML tags.

<histories>
{{#histories#}}
</histories>


Human: {{#query#}}

Assistant: 
```

**模板结构（Prompt Structure）：**

* 上下文（`Context`）
* 预编排提示词（`Pre-prompt`）
* 会话历史（`History`）
* 查询变量（`Query`）

### 4. 使用文本补全模型构建文本生成型应用模版

```Python
Use the following context as your learned knowledge, inside <context></context> XML tags.

<context>
{{#context#}}
</context>

When answer to user:
- If you don't know, just say that you don't know.
- If you don't know when you are not sure, ask for clarification.
Avoid mentioning that you obtained the information from the context.
And answer according to the language of the user's question.

{{pre_prompt}}
{{query}}
```

**模板结构（Prompt Structure）：**

* 上下文（`Context`）
* 预编排提示词（`Pre-prompt`）
* 查询变量（`Query`）

{% hint style="warning" %}
Dify 与部分模型厂商针对系统提示词做了联合深度优化，部分模型下的初始模版可能与以上示例不同。
{% endhint %}

### 参数释义

* 上下文（`Context`）：用于将数据集中的相关文本作为提示词上下文插入至完整的提示词中。
* 对话前提示词（`Pre-prompt`）：在**简易模式**下编排的对话前提示词将插入至完整提示词中。
* 会话历史（`History`）：使用文本生成模型构建聊天应用时，系统会将用户会话历史作为上下文插入至完整提示词中。由于部分模型对角色前缀的响应有所差异，你也可以在对话历史的设置中修改对话历史中的角色前缀名，例如：将 “`Assistant`” 改为 “`AI`”。
* 查询内容（`Query`）：查询内容为变量值，用于插入用户在聊天中输入的问题。
