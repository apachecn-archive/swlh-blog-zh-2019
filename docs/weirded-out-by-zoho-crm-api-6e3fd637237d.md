# 被 Zoho CRM API Auth 搞怪？

> 原文：<https://medium.com/swlh/weirded-out-by-zoho-crm-api-6e3fd637237d>

好吧，Zoho 文档不是很有帮助，我已经绞尽脑汁试图让我的节点应用程序与 Zoho APIs 对话。我不知道是谁决定将 OAuth 用于他们的 API 访问，我确信这有令人信服的理由，但这对于现代水疗来说是一件痛苦的事情。这里是一个一步一步的为同道奋斗。

![](img/84b694aae966ecd5e7432862bb6b6b29.png)

**第一步:安装 SDK**

*我们不打算用官方的 SDK，因为不管出于什么原因，Zoho 认为世界上只有一个存储引擎。假设我们使用 Node，我们甚至不需要运行 MySQL 来存储访问令牌——我们可以将它们保存在内存中。*

```
yarn add @trifoia/zcrmsdk
```

**第二步:API 令牌呼啦圈**

去[https://accounts.zoho.com/developerconsole](https://accounts.zoho.com/developerconsole)和**注册一个新的 app** 。(出于我们的目的，重定向 URL 可以是任意的，但是您需要在您的客户端设置中使用相同的 URL)。

我们现在需要生成一个**授权令牌**。点击新创建的应用程序的省略号菜单项，并选择自客户端。输入你需要的作用域(在这里看到全部[https://www . zoho . com/CRM/help/developer/API/oauth-overview . html # scopes](https://www.zoho.com/crm/help/developer/api/oauth-overview.html#scopes)，别忘了加上前缀 ZohoCRM 我认为`ZohoCRM.modules.all`对于大多数目的来说应该足够了)，选择一个更长的到期时间，复制生成的代码。

现在，在授权令牌过期之前，您需要抓紧时间生成一个 **referesh 令牌**。这是文档中真正神秘的部分，也是最令人悲伤的地方。他们所谓的`refresh_token`是一个持久令牌，可以用来永久地生成访问令牌，即 a-la API 密钥。

使用邮差或任何东西向:[https://accounts.zoho.com/oauth/v2/token?code=**<grant _ code>**&grant _ type = authorization _ code&client _ id =**<client _ id>**&client _ secret =**<client _ secret>**&redirect _ uri =**<redirect _ uri>**&response _ type = code](https://accounts.zoho.com/oauth/v2/token?code=1000.1c67743213ac86712e380e165ce59210.449fbcbe21691400345a854f601aca17&grant_type=authorization_code&client_id=1000.IWIT4FP8NZMU989528SIY22IOJXBRH&client_secret=72eb5825bb7a7f11ff04ac698718c5c3cf46661920&redirect_uri=http://localhost:3000/callback&response_type=code&access_type=offline)

您将收到一个带有 JSON 对象的响应，记下 **refresh_token** ，我们将需要它来设置我们的客户端。

**第三步:设置你的客户端**

我使用的是 express，但是您可以根据您使用的任何路由框架进行调整。

因此，首先您创建一个`zoho.config.js`来导出您的配置。更多信息见[https://www.npmjs.com/package/@trifoia/zcrmsdk](https://www.npmjs.com/package/@trifoia/zcrmsdk)。你可能会想用一些。env 变量，忽略 Zoho 用。属性文件。

**第四步:进行 API 调用**

现在您可以导入您的配置并开始进行 API 调用。

这个应该可以了。我希望这是有帮助的，你不要像我一样在 Zoho 的 OAuth 文档上浪费时间——它们不是为人们准备的，他们只需要通过 API 访问他们的 Zoho CRM 帐户，没有令牌交换、范围和 OAuth 带来的所有其他复杂性。