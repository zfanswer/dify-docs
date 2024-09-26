# 如何让 LLM 应用提供循序渐进的聊天体验？

让 LLM 应用提供循序渐进的聊天体验的关键在于，LLM 自身能够感知到与用户所处的对话轮数。例如在第五轮对话时深入扩展某项话题，或者在第 X 轮对话自动回顾历史对话并给出复盘分析。

本文将为你介绍如何使用 Chatflow 类型应用预置的 `sys.dialogue_count` 系统变量，利用其会随着对话轮数自动新增 +1 的特性，编排出能够感知对话轮数，并能够向用户提供循序渐进聊天体验的 AI 应用。

### 场景 1：为用户提供循序渐进的对话深度

**应用场景：AI 编程教师**

新手刚开始学习编程知识时，初始阶段大量晦涩的专业名词将造成困扰。一个理想的学习场景是老师能够根据学生自身对知识的掌握情况，以及对话的聊天次数，循序渐进介绍更多教学内容并布置合理的学习任务。

#### 设计思路

| 对话轮数    | 教学策略       | 内容重点                    |
| ------- | ---------- | ----------------------- |
| 1-5 轮   | 使用简单、易懂的语言 | 介绍编程的基本概念（变量、数据类型、控制流等） |
| 6-10 轮  | 逐步引入基础编程术语 | 深入探讨基础概念，提供更多示例         |
| 11-15 轮 | 使用更多专业术语   | 探讨特定编程主题（如面向对象编程、数据结构）  |
| 15 轮以后  | 高级编程对话     | 讨论高级概念、最佳实践、项目开发        |

#### 实现方法

1. 设立第一个 LLM 节点，收集并分析用户的初始编程水平能力
2. 新增数个条件分支，判断该用户与第一个 LLM 节点的对话次数
3. 将用户分流至不同阶段编程学习 LLM 节点。

在第一个 LLM 节点后新增数个条件分支，判断 `sys.dialogue_count` 值的所处区间，然后分流至不同编程阶段的 LLM 节点。

<figure><img src="https://langgenius.feishu.cn/space/api/box/stream/download/asynccode/?code=MzNlZjFlY2M5OTNmMmJlNmNhZmIwOWU0Y2VjZTRiZjJfZ0xsRm9BZHdRODJJa1VkblR1VWhnbm5KOE9ZdXFNcHNfVG9rZW46U25yVmJnT3Iyb0VPYVZ4RWF1d2NWQThUbk5mXzE3MjQ4MjU3ODc6MTcyNDgyOTM4N19WNA" alt=""><figcaption><p>AI 编程教师</p></figcaption></figure>

### 场景 2：定期回顾对话历史

**应用场景：语言能力测试 AI 助手**

在学习新语言时，定期复习和巩固知识点对于长期记忆至关重要。AI 语言学习助手可以通过跟踪对话轮数，在适当的时机提供回顾和测试。

#### 设计思路

| 对话轮数   | 学习策略 | 活动类型                           | 目的        |
| ------ | ---- | ------------------------------ | --------- |
| 每 10 轮 | 知识回顾 | 简短复习测验                         | 巩固近期学习的内容 |
| 每 20 轮 | 综合测试 | 全面的语言能力测试，然后给出能力评估报告以及接下来的学习建议 | 评估整体学习进度  |

#### 实现方法

1. 设立第一个 LLM 对话应用，收集并分析用户的初始语言能力，并给出训练习题
2. 新增条件分支，在第 10 轮对话时制订小型测验，并给出学习回顾；在第 20 轮对话时给出更加全面的测验和学习报告。其余对话轮数则正常给出单个训练习题。

通过在特定轮数回顾学生过往的学习分析报告，LLM 能够更加像一个专业老师一样重新审视并调整用户的学习计划。

<figure><img src="https://langgenius.feishu.cn/space/api/box/stream/download/asynccode/?code=ZjdjMjNmZmE4YzUzOWUwNDk5NjRkNzBkNjcxMzZiY2NfSENEZVZ1RFVnTkpGNTBESUVrVEtQVXZVUEdpMEcyOEZfVG9rZW46UlNMQmJSeG5Sb0pHVGF4U3FBQmNzSUlybjZkXzE3MjQ4MjU3ODc6MTcyNDgyOTM4N19WNA" alt=""><figcaption></figcaption></figure>

> 如果还想要了解更多关于工作流的编排技巧，请参考[《工作流》](https://docs.dify.ai/v/zh-hans/guides/workflow)。
