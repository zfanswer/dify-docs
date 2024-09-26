# 负载均衡

模型速率限制（Rate limits）是模型厂商对用户或客户在指定时间内访问 API 服务次数所添加的限制。它有助于防止 API 的滥用或误用，有助于确保每个用户都能公平地访问 API，控制基础设施的总体负载。

在企业级大规模调用模型 API 时，高并发请求会导致超过请求速率限制并影响用户访问。负载均衡可以通过在多个 API 端点之间分配 API 请求，确保所有用户都能获得最快的响应和最高的模型调用吞吐量，保障业务稳定运行。

你可以在 **模型供应商 -- 模型列表 -- 设置模型负载均衡** 打开该功能，并在同一个模型上添加多个凭据 (API key)。

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption><p>模型负载均衡</p></figcaption></figure>

{% hint style="info" %}
模型负载均衡为付费特性，您可以通过[订阅 SaaS 付费服务](../../getting-started/cloud.md#ding-yue-ji-hua)或者购买企业版来开启该功能。
{% endhint %}

默认配置中的 API Key 为初次配置模型供应商时添加的凭据，您需要点击 **增加配置** 添加同一模型的不同 API Key 来正常使用负载均衡功能。

<figure><img src="../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption><p>配置负载均衡</p></figcaption></figure>

**需要额外添加至少 1 个模型凭据**即可保存并开启负载均衡。

你也可以将已配置的凭据**临时停用**或者**删除**。

<figure><img src="../../.gitbook/assets/image (7) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

配置完成后再模型列表内会显示所有已开启负载均衡的模型。

<figure><img src="../../.gitbook/assets/image (6) (1) (1) (1).png" alt="" width="563"><figcaption><p>开启负载均衡</p></figcaption></figure>

{% hint style="info" %}
默认情况下，负载均衡使用 Round-robin 策略。如果触发速率限制，将应用 1 分钟的冷却时间。
{% endhint %}

你也可以从 **添加模型** 配置负载均衡，配置流程与上面一致。

<figure><img src="../../.gitbook/assets/image (4) (1) (1) (1).png" alt="" width="563"><figcaption><p>从添加模型配置负载均衡</p></figcaption></figure>
