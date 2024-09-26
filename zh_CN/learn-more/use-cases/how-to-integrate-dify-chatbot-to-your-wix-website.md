# 如何将 Dify Chatbot 集成至 Wix 网站？

Wix 是一个非常流行的网站创建平台，它允许用户通过拖拽的方式可视化创建自己的网站。通过 iframe 代码功能，能够实现 Wix 与 Dify 聊天机器人的集成。

除此之外，你还可以使用此功能在 Wix 网页内显示来自外部服务器和其他来源的内容，例如说创建天气小部件、股票小部件、日历或任何其它自定义网页元素。

在本文中，我们将指导你如何将 Dify 聊天机器人通过 iframe 代码嵌入到你的 Wix 网站中。除了将应用嵌入至 Wix 网站内，你也可以通过同样的方法将 Dify 应用集成至你的网站、博客或其他网页中。

## 1. 获取 Dify 应用的 iFrame 代码片段

假设你已创建了一个 [Dify AI 应用](https://docs.dify.ai/v/zh-hans/guides/application-orchestrate/creating-an-application)，你可以通过以下步骤获取 Dify 应用的 iFrame 代码片段：

- 登录你的 Dify 账户
- 选择你想要嵌入的 Dify 应用
- 点击右上角的“发布”按钮
- 在发布页面中，选择 “Embed Into Site” 选项。
  
  ![](../../../img/best-practice-wix-2.png)

- 选择合适的样式并复制显示的 iFrame 代码，例如：

![](../../../img/best-practice-wix-3.png)

## 2. 在 Wix 网站中嵌入 iFrame 代码片段

登录 Wix 网站，打开你要编辑的网页。点击网页左侧蓝色的 `+` （Add Elements）按钮，然后选择 **Embed Code**，最后点击 **Embed HTML** 添加一个 HTML iFrame 元素到页面。

![](../../../img/best-practice-add-html-iframe.png)

在 `HTML Settings` 框内选择 `Code` 选项，复制并粘贴你在 Dify 应用中获取的 iFrame 代码片段，然后点击 **Update** 按钮进行保存并进行预览。

以下是一个嵌入 Dify Chatbot 的 iFrame 代码示例：

```bash
<iframe src="https://udify.app/chatbot/1yS3gohroW1sKyLc" style="width: 100%; height: 100%; min-height: 700px" frameborder="0" allow="microphone"></iframe>
```

![](../../../img/best-practice-insert-dify-iframe-code.png)

> ⚠️ 请确保 iFrame 代码中的地址以 HTTPS 开头，HTTP 地址将无法正常显示。

## 3. 自定义 Dify Chatbot

你可以调整 Dify Chatbot 的按钮样式、位置和其它设置。

### 3.1 自定义样式

你可以通过修改 iFrame 代码中的 `style` 属性来自定义 Chatbot 按钮的样式。例如：

```bash
<iframe src="https://udify.app/chatbot/1yS3gohroW1sKyLc" style="width: 100%; height: 100%; min-height: 700px" frameborder="0" allow="microphone"></iframe>

# 添加一个 2 像素宽的黑色实线边框 border: 2px solid #000

→

<iframe src="https://udify.app/chatbot/1yS3gohroW1sKyLc" style="width: 80%; height: 80%; min-height: 500px; border: 2px solid #000;" frameborder="0" allow="microphone"></iframe>
```

### 3.2 自定义位置

你还可以通过修改 `style` 属性中的 `position` 值可以调整按钮的位置。例如：

```bash
<iframe src="https://udify.app/chatbot/1yS3gohroW1sKyLc" style="width: 100%; height: 100%; min-height: 700px" frameborder="0" allow="microphone"></iframe>

# 将 Chatbot 固定在网页右下角，距离底部和右侧 20 像素。

→

<iframe src="https://udify.app/chatbot/1yS3gohroW1sKyLc" style="width: 100%; height: 100%; min-height: 700px; position: fixed; bottom: 20px; right: 20px;" frameborder="0" allow="microphone"></iframe>
```

## 常见问题

**1. iframe 方框内未正常显示内容应如何处理？**

- 确保 URL 以 HTTPS 开头；
- 检查 `iframe` 代码中是否有拼写错误；
- 确认嵌入的内容符合 Wix 的安全政策；

**2. iframe 内容被裁剪，内容显示不全应如何处理？**

你可以调整并修改 `iframe` 代码框内的 `width` 和 `height` 的百分比值。
