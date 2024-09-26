# 使用 Dify 和 Azure Bot Framework 构建 Microsoft Teams 机器人

> 作者：Warren， [Microsoft 最有价值专家 (MVP)](https://mvp.microsoft.com/en-US/mvp/profile/476f41d3-6bd1-ea11-a812-000d3a8dfe0d)

## 1. 概述

随着人工智能的崛起，聊天机器人变得更聪明，更个性化，更直观。在本文中，我们将向你展示如何使用 Azure Bot Service，将 Dify 集成至 Microsoft Teams，打造团队中的 AI 助手。

在本文中将介绍如何集成 Azure Bot Service 的 Teams Channel，允许客户与你的 Microsoft Teams 聊天机器人开始对话。

## 2. 准备工作

- 安装好 Docker 和 Docker Compose
- Microsoft Teams 账户
- 一个安装了 Microsoft Teams 的手机或电脑，用于测试你的 AI 聊天机器人
- [Azure 账户](https://azure.microsoft.com/en-us/free)

## 3. 创建 Dify 基础编排聊天助手应用

首先，登录 [Dify 平台](https://cloud.dify.ai/signin)，使用 Github 登录或者使用 Google 登录。此外，你也可以参考 Dify 官方教程 [Docker Compose 部署](https://docs.dify.ai/v/zh-hans/getting-started/install-self-hosted/docker-compose) 私有部署。

<figure><img src="../../.gitbook/assets/dify-on-wechat/create-basic-chatbot.jpg" alt=""><figcaption></figcaption></figure>

登录成功后，进入 Dify 页面，我们按照下方步骤创建一个基础编排聊天助手应用

1. 点击页面上方的工作室
2. 创建空白应用
3. 应用类型选择聊天助手
4. 聊天助手编排方式选择基础编排
5. 选择应用图标并为应用填写一个名称，比如基础编排聊天助手
6. 点击创建

<figure><img src="../../.gitbook/assets/dify-on-wechat/config-basic-chatbot.jpg" alt=""><figcaption></figcaption></figure>

创建成功后会跳转到上图所示页面，此时继续配置应用

1. 选择模型，如 gpt-3.5-turbo-0125
2. 设置模型参数
3. 填写应用提示词
<figure><img src="../../.gitbook/assets/dify-on-wechat/publish-basic-chatbot.jpg" alt=""><figcaption></figcaption></figure>

配置完成后，可以在右侧对话框进行测试。测试完成后，进行如下操作：

1. 发布
2. 更新
3. 访问 API

接着生成基础编排聊天助手 API 密钥

<figure><img src="../../.gitbook/assets/dify-on-wechat/create-basic-chatbot-apikey.jpg" alt=""><figcaption></figcaption></figure>

点击 "访问 API" 后，会跳转到上图的 API 管理页面，按照如下步骤获取 API 密钥：

1. 点击右上角 API 密钥
2. 点击创建密钥
3. 复制保存密钥

在保存密钥后，还需要查看右上角的 API 服务器，如果是 Dify 官网的应用，API 服务器地址为 `https://api.dify.ai/v1`, 如果是私有部署的，请确认你自己的 API 服务器地址。

至此，创建聊天助手的准备工作结束。在此小节中我们只需要保存好两个东西：**API 密钥** 与 **API 服务器地址**。

## 4. 创建 Azure Bot Service

转到 [Azure Marketplace](https://portal.azure.com/#view/Microsoft_Azure_Marketplace/GalleryItemDetailsBladeNopdl/id/Microsoft.AzureBot/selectionMode~/false/resourceGroupId//resourceGroupLocation//dontDiscardJourney~/false/selectedMenuId/home/launchingContext~/%7B%22galleryItemId%22%3A%22Microsoft.AzureBot%22%2C%22source%22%3A%5B%22GalleryFeaturedMenuItemPart%22%2C%22VirtualizedTileDetails%22%5D%2C%22menuItemId%22%3A%22home%22%2C%22subMenuItemId%22%3A%22Search%20results%22%2C%22telemetryId%22%3A%22a09b3b54-129b-475f-bd39-d7285a272043%22%7D/searchTelemetryId/258b225f-e7d5-4744-bfe4-69fa701d9d5a) 画面应该会直接到 Azure Bot Service 的创建画面。

<figure><img src="../../.gitbook/assets/dify-on-teams/azure1.png" alt=""><figcaption></figcaption></figure>

确认所有设置然后创建。

<figure><img src="../../.gitbook/assets/dify-on-teams/azure2.png" alt=""><figcaption></figcaption></figure>

创建好后转到 Azure Bot 页面，先把 Subscription ID 保存好。

<figure><img src="../../.gitbook/assets/dify-on-teams/azure3.png" alt=""><figcaption></figcaption></figure>

然后选择 Configuration，保存好 Microsoft App ID 然后选择 Manage Password。

<figure><img src="../../.gitbook/assets/dify-on-teams/azure4.png" alt=""><figcaption></figcaption></figure>

创建和保存新 client secret。

<figure><img src="../../.gitbook/assets/dify-on-teams/azure5.png" alt=""><figcaption></figcaption></figure>

## 5. 创建你的聊天机器人

在这一部分，你将使用 Microsoft Bot Framework 编写一个基本的聊天机器人的代码。

#### 5.1 下载代码

```
git clone https://github.com/somethingwentwell/dify-teams-bot
```

#### 5.2 配置.env

在项目根目录创建.env，内容如下:

```
MicrosoftAppType=MultiTenant
MicrosoftAppId=< 在 (4) 获取的 Client ID>
MicrosoftAppPassword=< 在 (4) 获取的 Client Secret>
MicrosoftAppTenantId=< 在 (4) 获取的 Tenant ID>

API_ENDPOINT=< 在 (3) 获取的 Dify API 服务器地址 >
API_KEY=< 在 (3) 获取的 Dify API 密钥 >
```

#### 5.3 运行代码

执行 docker compose up
```
docker compose up
```

如果运行成功，你应该会看到
```
[+] Running 2/0
 ✔ Network dify-teams-bot_dify-network    Created                     0.0s 
 ✔ Container dify-teams-bot-nodejs-app-1  Created                     0.0s 
Attaching to nodejs-app-1
nodejs-app-1  | 
nodejs-app-1  | > echobot@1.0.0 start
nodejs-app-1  | > node ./index.js
nodejs-app-1  | 
nodejs-app-1  | (node:18) [DEP0111] DeprecationWarning: Access to process.binding ('http_parser') is deprecated.
nodejs-app-1  | (Use `node --trace-deprecation ...` to show where the warning was created)
nodejs-app-1  | (node:18) [DEP0111] DeprecationWarning: Access to process.binding ('http_parser') is deprecated.
nodejs-app-1  | 
nodejs-app-1  | restify listening to http://0.0.0.0:3978
nodejs-app-1  | 
nodejs-app-1  | Get Bot Framework Emulator: https://aka.ms/botframework-emulator
nodejs-app-1  | 
nodejs-app-1  | To talk to your bot, open the emulator select "Open Bot"
```

#### 5.4 使用 Localtunnel 将本地项目放到公网访问

Azure Bot 需要向你的后端发送消息，你需要在公共服务器上托管你的应用。一个简单的方法是使用 localtunnel。

让 Azure BotI 应用继续在 3978 端口运行，并在另一个终端窗口运行以下 localtunnel 命令：

```
npx localtunnel --port 3978
```

上述命令在你的本地服务器（运行在 3978 端口）和 localtunnel 创建的公共域之间建立了一个连接。一旦你有了 localtunnel 转发 URL，任何来自客户端对该 URL 的请求都会自动被定向到你的 FastAPI 后端。

<figure><img src="../../.gitbook/assets/dify-on-teams/lt1.png" alt=""><figcaption></figcaption></figure>


## 6. 配置你的 Azure Bot

#### 6.1 打开 Azure Bot

转到在第四步创建的 Azure Bot，并在左侧面板上选择 Configuration。

将 `Message Endpoint` 替换为在第五步中获取的 localtunnel 转发 URL。

<figure><img src="../../.gitbook/assets/dify-on-teams/azure6.png" alt=""><figcaption></figcaption></figure>

#### 6.2 配置 Channels

在左侧面板上选择 Channels，点击 Microsoft Teams。

<figure><img src="../../.gitbook/assets/dify-on-teams/azure7.png" alt=""><figcaption></figcaption></figure>

## 7. Teams 测试

回到在 6.2 步中创建的 Channels 页面，点击 “Open in Teams”

<figure><img src="../../.gitbook/assets/dify-on-teams/azure8.png" alt=""><figcaption></figcaption></figure>

发送 Teams 消息，并等待你的 AI 聊天机器人的回复。尝试向 AI 聊天机器人提问你可以向 Dify 聊天助手提问的任何问题。

<figure><img src="../../.gitbook/assets/dify-on-teams/teams1.png" alt=""><figcaption></figcaption></figure>

## 8. 后记

现在，你的 AI 聊天机器人应该能够在 Microsoft Teams 上运行良好。如果运行良好，可以切换至使用公司的 Microsoft Teams 商业账户内的正式环境，并使用远程服务器托管，使这个 Teams 助手在生产环境中运行。希望你喜欢这个教程，我们下次再见。
