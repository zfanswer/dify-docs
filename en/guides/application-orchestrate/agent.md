# Agent

## Definition

An Agent Assistant can leverage the reasoning abilities of large language models (LLMs). It independently sets goals, simplifies complex tasks, operates tools, and refines processes to complete tasks autonomously.

## Usage Instructions

To facilitate quick learning and use, application templates for the Agent Assistant are available in the 'Explore' section. You can integrate these templates into your workspace. The new Dify 'Studio' also allows the creation of a custom Agent Assistant to suit individual requirements. This assistant can assist in analyzing financial reports, composing reports, designing logos, and organizing travel plans.

<figure><img src="/en/.gitbook/assets/guides/application_orchestrate/agent/docs-1.png" alt=""><figcaption><p>Explore-Agent Assistant Application Template</p></figcaption></figure>

The task completion ability of the Agent Assistant depends on the inference capabilities of the model selected. We recommend using a more powerful model series like GPT-4 when employing Agent Assistant to achieve more stable task completion results.

<figure><img src="/en/.gitbook/assets/guides/application_orchestrate/agent/docs-3.png" alt=""><figcaption><p>Selecting the Reasoning Model for Agent Assistant</p></figcaption></figure>

You can write prompts for the Agent Assistant in 'Instructions'. To achieve optimal results, you can clearly define its task objectives, workflow, resources, and limitations in the instructions.

<figure><img src="/en/.gitbook/assets/guides/application_orchestrate/agent/docs-4.png" alt=""><figcaption><p>Orchestrating Prompts for Agent Assistant</p></figcaption></figure>

## Adding Tools for the Agent Assistant

In the "Context" section, you can incorporate knowledge base tools that the Agent Assistant can utilize for information retrieval. This will assist in providing it with external background knowledge.

In the "Tools" section, you are able to add tools that are required for use. These tools can enhance the capabilities of LLMs, such as internet searches, scientific computations, or image creation, thereby enriching the LLM's ability to interact with the real world. Dify offers two types of tools: **built-in tools and custom tools.**

You have the option to directly use built-in tools in Dify, or you can easily import custom API tools (currently supporting OpenAPI/Swagger and OpenAI Plugin standards).

<figure><img src="/en/.gitbook/assets/guides/application_orchestrate/agent/docs-5.png" alt=""><figcaption><p>Adding Tools for the Assistant</p></figcaption></figure>

The **Tools** feature allows you to create more powerful AI applications on Dify. For example, you can orchestrate suitable tools for Agent Assistant, enabling it to complete complex tasks through reasoning, step decomposition, and tool invocation. 

Additionally, the tool simplifies the integration of your application with other systems or services, enabling interactions with the external environment, such as executing code or accessing proprietary information sources. Simply mention the name of the tool you want to invoke in the chat box, and it will be automatically activated.

![](../../../img/agent-dalle3.png)

## Agent Settings

On Dify, two inference modes are provided for Agent Assistant: Function Calling and ReAct. Models like GPT-3.5 and GPT-4 that support Function Calling have demonstrated better and more stable performance. For model series that do not support Function Calling, we have implemented the ReAct inference framework to achieve similar effects.&#x20;

In the Agent settings, you can modify the iteration limit of the Agent.

<figure><img src="/en/.gitbook/assets/guides/application_orchestrate/agent/docs-6.png" alt=""><figcaption><p>Function Calling Mode</p></figcaption></figure>

<figure><img src="/en/.gitbook/assets/guides/application_orchestrate/agent/sec-7.png" alt=""><figcaption><p>ReAct Mode</p></figcaption></figure>

## Configuring the Conversation Opener

You can set up a conversation opener and initial questions for your Agent Assistant. The configured conversation opener will be displayed at the beginning of each user's first interaction, showcasing the types of tasks the Agent can perform, along with examples of questions that can be asked.

<figure><img src="/en/.gitbook/assets/guides/application_orchestrate/agent/docs-8.png" alt=""><figcaption><p>Configuring the Conversation Opener and Initial Questions</p></figcaption></figure>

## Debugging and Preview

After orchestrating your Agent Assistant, you have the option to debug and preview it before publishing it as an application. This allows you to assess the effectiveness of the agent in completing tasks.

<figure><img src="/en/.gitbook/assets/guides/application_orchestrate/agent/docs-9.png" alt=""><figcaption><p>Debugging and Preview</p></figcaption></figure>

## Application Publish

<figure><img src="/en/.gitbook/assets/guides/application_orchestrate/agent/docs-10.png" alt=""><figcaption><p>Publishing the Application as a Webapp</p></figcaption></figure>
