# Perplexity Search

> Tool author @Dify.

Perplexity is an AI-based search engine that can understand complex queries and provide accurate, relevant real-time answers. Here are the steps to configure and use the Perplexity Search tool in Dify.

## 1. Apply for Perplexity API Key

Please apply for an API Key at [Perplexity](https://www.perplexity.ai/settings/api), and ensure that your account has sufficient Credits.

## 2. Fill in the configuration in Dify

In the Dify navigation page, click on `Tools > Perplexity > Go to authorize` to fill in the API Key.

![](../../../../img/tools-perplexity.png)

## 3. Using the tool

You can use the Perplexity Search tool in the following application types.

- **Chatflow / Workflow applications**

Both Chatflow and Workflow applications support adding Perplexity tool nodes. Pass the user's input content through variables to the "Query" box in the Perplexity tool node, adjust the built-in parameters of the Perplexity tool as needed, and finally select the output content of the Perplexity tool node in the response box of the "End" node.

![](../../../../img/tools-chatflow-perplexity.png)

- **Agent applications**

Add the `Perplexity Search` tool in the Agent application, then enter relevant commands to invoke this tool.

![](../../../../img/tools-agent-perplexity.png)
