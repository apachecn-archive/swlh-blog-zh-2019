# 亚马逊木偶师

> 原文：<https://medium.com/swlh/amazonian-puppeteer-b590d199dfdc>

让木偶师在 AWS Lambda 中工作的冒险

![](img/53aa0638055ba1ff9943be13c6242dd6.png)

[https://www.thegef.org/sites/default/files/Amazon_870.jpg](https://www.thegef.org/sites/default/files/Amazon_870.jpg)

# 介绍

我是我认识的最健忘的人。毫不奇怪，到了周末提交时间表的时候，我似乎总是忘记。好吧，我决定自己动手，把整件该死的事情自动化。

这个想法是屏幕抓取我的时间记录应用程序，因为他们不提供一个 API。然后获取该信息并提交给另一个时间表应用程序(也没有 API)。我提到这些细节只是作为证明围绕该技术的决策的前言。

归根结底，我需要自动执行每周提交，所以我需要某种 cron 作业。它将从 javascript 动态生成的屏幕抓取数据，所以我需要一个无头浏览器。我正在使用我的用户名和密码，所以我需要某种秘密存储。

技术堆栈 AWS Lambda、CloudWatch cron event、AWS Secrets Manager 和木偶师中的选择。我不会详细说明我是如何让我的最终解决方案发挥作用的，而是会向您展示我的技术堆栈的工作实现。

# 要求

在我们真正开始之前，我们需要事先了解一些事情。

1.  AWS 账户:不言而喻，如果你要使用 AWS，你需要有一个账户。我们将尽可能利用免费层服务。[https://AWS . Amazon . com/premium support/knowledge-center/create-and-activate-AWS-account/](https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account/)
2.  **安装 docker**:AWS 提倡的开发方式挺有意思的。为了模拟 Lambda 运行时环境，AWS 提供了许多 Docker 映像供开发人员使用，这意味着我们需要 Docker 来使用它们。对于 Mac 系统[https://AWS . Amazon . com/premium support/knowledge-center/create-and-activate-AWS-account/](https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account/)和 Windows 系统[https://docs.docker.com/docker-for-windows/install/系统](https://docs.docker.com/docker-for-windows/install/)。我用的版本是 Mac 2.0.0.3 的 Docker 桌面。
3.  **安装 Node JS** :因为应用程序将使用 Node JS，所以确保您已经安装了 Node JS 和 NPM 的最新版本是很有意义的。我用的版本是 Node v11.9.0 和 NPM v 6.9.0。[https://nodejs.org/en/download/](https://nodejs.org/en/download/)
4.  **安装 AWS 命令行界面**:我们将使用 AWS 无服务器应用程序模型来创建和提交 lambda 函数，这些函数又依赖于 AWS Cli。这意味着还需要安装 Python 和 PIP。[https://docs . AWS . Amazon . com/CLI/latest/user guide/CLI-chap-install . html](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html)要获得您正在使用的版本，您可以键入“aws - version ”,对我来说是:AWS-CLI/1 . 16 . 99 Python/2 . 7 . 15 Darwin/18 . 6 . 0 boto core/1 . 12 . 89
5.  **安装 SAM 命令行界面**:我决定利用无服务器应用模型来提交和管理 lambda 函数，因为它将所有东西都保存在 AWS 堆栈中，并且我可以利用我对 CloudFormation 模板的熟悉程度。https://docs . AWS . Amazon . com/server less-application-model/latest/developer guide/server less-SAM-CLI-install . html 对我来说这就是 SAM CLI，版本 0.16.1。

# 地形* * TL 灾难恢复警告**

在我进入这个过程之前，我想简单介绍一下我使用的技术和概念。我觉得对于那些不知道或者忘记了，需要提醒的人来说是公平的。公平的警告，虽然这是 TL；如果你已经很熟悉这个东西，你可以跳过这个。

*   **Lambda** : Lambda 是一款 AWS 无服务器产品。它是在你为你所使用的东西付费的基础上运作的。您基本上创建并上传了您期望执行的函数，并指定了它应该如何被调度，但是并不确定它将在实际的硬件上运行。Lambda 是以 GB 秒为基础定价的，这意味着您需要为每个 Lambda 函数的持续时间内提供的千兆字节数付费，无论这些千兆字节是否被使用。Lambda 可以在许多运行时环境 NodeJS、Python、.网络等。像大多数 AWS 服务一样，它连接到 AWS 基础设施。[https://aws.amazon.com/lambda/](https://aws.amazon.com/lambda/)
*   **cloud formation**:cloud formation 是 AWS 将其基础设施指定为代码的方式。它使用 yaml 或 json 格式来声明性地定义要提供哪些资源和基础设施。通过 AWS 中的 CloudFormation，您可以定义想要使用的完整资源堆栈。它允许以幂等的方式提供资源。它使用模板引用、资源引用和参数。[https://aws.amazon.com/cloudformation/](https://aws.amazon.com/cloudformation/)
*   SAM 模板:SAM 模板是 CloudFormation 的扩展，这意味着您可以使用全套资源。它附带了一些专门用于定义无服务器应用程序的简化结构。[https://docs . AWS . Amazon . com/server less-application-model/latest/developer guide/server less-Sam-template-basics . html](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-template-basics.html)
*   **Cron expression**:Cron expression 源于 unix 系统中的一个实用程序，它允许用户安排任务在指定的日期/时间定期运行。cron 语法表示一个描述事件何时执行的表达式。它通常由六个必填字段组成，其中<年>是可选的([https://www.baeldung.com/cron-expressions](https://www.baeldung.com/cron-expressions)):

> 【T8<minute><hour><day-of-month><month><day-of-week><year></year></day-of-week></month></day-of-month></hour></minute>

*   **Chrome DevTools 协议**:这是一个允许工具对 Chrome、Chrome 和其他基于 Blink 的浏览器进行检测、检查、调试和分析的协议。该协议给出了集成和调试浏览器的标准化方法[https://chromedevtools.github.io/devtools-protocol/](https://chromedevtools.github.io/devtools-protocol/)
*   Chromium 是一个开源浏览器项目，它构成了 Chrome 网络浏览器的基础，不需要额外的东西，比如自动更新和对额外视频格式的支持。鉴于 Chromium 支持 Chrome DevTools 协议，它是无头浏览器的完美候选。一个[无头**浏览器**](https://en.wikipedia.org/wiki/Headless_browser)基本上是一个没有图形界面的网络浏览器。[https://www . how togeek . com/202825/what % E2 % 80% 99s-the-difference-of-chromium-and-chrome/](https://www.howtogeek.com/202825/what%E2%80%99s-the-difference-between-chromium-and-chrome/)
*   **木偶师**:木偶师是一个节点库，它提供了一个高级 API 来控制 Chrome 或通过 [DevTools 协议](https://chromedevtools.github.io/devtools-protocol/)的 Chrome。默认情况下，木偶师运行无头的[，但可以配置为运行全(无头)铬或铬。在本文中，我使用了 puppeter-core，它是相同的 api，但默认情况下不下载 chromium 浏览器。](https://developers.google.com/web/updates/2017/04/headless-chrome)[https://github.com/GoogleChrome/puppeteer](https://github.com/GoogleChrome/puppeteer)
*   AWS Secrets Manager :这是 AWS 提供的一项服务，用于帮助管理受保护的机密。它允许我们使用证书在 AWS 中存储敏感信息。使用 Secrets Manager，我们可以将加密和证书轮换等管理细节留给 AWS。[https://aws.amazon.com/secrets-manager/](https://aws.amazon.com/secrets-manager/)

# 冒险

一旦我安装并配置好了所有东西，我就通读了一下 [SAM 文档](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-quick-start.html)，看起来开始一切只是一个简单的 SAM 初始化。这里有几个注意事项，首先是你需要以 [nodejs8.10](https://github.com/alixaxel/chrome-aws-lambda#usage) 运行时为目标，因为这是 [chrome-aws-lambda](https://github.com/alixaxel/chrome-aws-lambda) 包所需的运行时。第二，你可能不想把你的工作文件夹命名为 hello-world，所以给它一个名字是个好主意。

```
sam init -r nodejs8.10 -n autoweb
```

*   -r 是运行时的缩写
*   -n 是名字的缩写
*   您可以使用“sam init - help”获得帮助

生成的自述文件很好地解释了正在发生的一切，所以我不打算进一步展开。default template.yaml 文件中的链接也非常好，所以最好记下它们并保存到您的收藏夹中以备后用。以防万一，我将在下面列出它们。

*   关于全局的更多信息:[https://github . com/aw slabs/server less-application-model/blob/master/docs/Globals . rst](https://github.com/awslabs/serverless-application-model/blob/master/docs/globals.rst)
*   关于函数资源的更多信息:[https://github . com/aw slabs/server less-application-model/blob/master/versions/2016-10-31 . MD # awsserverlessfunction](https://github.com/awslabs/serverless-application-model/blob/master/versions/2016-10-31.md#awsserverlessfunction)
*   关于 API 事件源的更多信息:[https://github . com/aw slabs/server less-application-model/blob/master/versions/2016-10-31 . MD # API](https://github.com/awslabs/serverless-application-model/blob/master/versions/2016-10-31.md#api)
*   ServerlessRestApi 是在 Serverless::Function 下用 Events 键创建的隐式 Api。了解更多关于您可以在 SAM 中引用的其他隐式资源:[https://github . com/aw slabs/server less-application-model/blob/master/docs/internals/generated _ resources . rst # API](https://github.com/awslabs/serverless-application-model/blob/master/docs/internals/generated_resources.rst#api)

拥有一个默认的入门模板是很棒的，但是我们有一个名为 hello-world 的文件夹。我们也没有使用 api 端点，至少在这个例子中没有，所以让我们来解决这个问题。在 template.yaml 中，对全局变量下面的资源部分进行更改。

```
Resources:
   AutoWebFunction:
   Type: AWS::Serverless::Function
   Properties:
      CodeUri: auto-web/
      Handler: app.lambdaHandler
      Runtime: nodejs8.10Outputs:
   AutoWebFunction:
      Description: "Hello World Lambda Function ARN"
      Value: !GetAtt AutoWebFunction.Arn
   AutoWebFunctionIamRole:
      Description: "Implicit IAM Role created"
      Value: !GetAtt AutoWebFunctionRole.Arn
```

在 CloudFormation 模板中，您可以直接在 resources 部分下面指定您正在使用的资源的名称。在我们的例子中，我们不再想称它为 HelloWorldFunction，而是称它为 AutoWebFunction。CodeUri 属性指定了 lambda 函数的文件夹位置，在这种情况下，它的文件夹位置是相对于我们希望成为 auto-web/的 template.yaml 文件的。我们现在可以删除 Events 部分，因为我们没有使用 API 端点。由于资源部分已经改变，我们需要在输出部分反映新的参考值。最后，API 输出也可以删除，因为我们在这个例子中没有使用 Api。要重命名文件夹，我们可以使用 mv bash 命令。

```
mv hello-world auto-web
```

看起来不错，但是让我们开始吧。再次快速回头看一下[文档](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-quick-start.html)，看起来要运行的命令是 sam local。因为我们没有使用 API，所以我们需要用我们想要调用的函数名来指定 invoke 参数，在本例中是 AutoWebFunction。

```
sam local invoke AutoWebFunction
```

该死，它在提醒我。如果我仔细看提示，它说还有另一个选项，使用输入文件代替。如果我查看文件和文件夹，我会注意到同一个文件夹中有一个 event.json 文件。好了，现在我们有东西可以用了。

```
sam local invoke AutoWebFunction --event event.json
```

嗯，我可以运行它，但我如何调试它。再看一下[文档](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-using-debugging.html)，我们有一个如何启用调试的纲要。我使用的是 [VSCode](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-using-debugging-nodejs.html) ，所以我需要创建一个定制的启动配置并选择一个端口号。为此，我选择调试菜单>在 VSCode 中添加配置…,默认情况下会创建一个。工作文件夹中的 vscode/launch.json 文件。下面是我的 launch.json 的样子:

```
{
"version": "0.2.0",
"configurations": [
 {
  "type": "node",
  "request": "attach",
  "name": "Attach to AutoWeb",
  "address": "localhost",
  "port": 9229,
  "localRoot": "${workspaceFolder}/auto-web",
  "remoteRoot": "/var/task",
  "protocol": "inspector",
  "stopOnEntry": false
 }
]
}
```

要进行调试，您需要首先启动调试器，并让它监听您在 launch.json 文件中指定的端口，在本例中是 9229。

```
sam local invoke AutoWebFunction --event event.json -d 9229
```

当它运行并侦听时，您可以使用刚刚创建的启动配置从 VSCode 启动调试器。不要忘记在。/auto-web/app.js 文件。

我们得到了函数并可以调试，我认为现在是一个进入函数细节的好时机。在我们继续之前，我觉得有必要指出一个关于 Lambda 的非常重要的细节，那就是对[部署包](https://docs.aws.amazon.com/lambda/latest/dg/deployment-package-v2.html)大小的限制，基本上你的包最大需要 [50 MB 压缩和 250 MB 解压缩](https://docs.aws.amazon.com/lambda/latest/dg/limits.html)。为什么我现在提到这个是因为我犯了一个错误，就是安装了木偶师。当我部署或测试时，我不知道哪里出了问题，它以意想不到的方式中断，在第一次等待时冻结，等等。当我进一步调查时，我发现默认情况下，木偶师安装了一个大小为 [~170 MB](https://www.npmjs.com/package/puppeteer#installation) 的 Chromium 版本。如你所见，这对于 Lambda 函数来说不是一个可行的选择。幸运的是，我发现了 chrome-AWS-lambda(T7 ),它的容量仍然高达 33.21 MB(T8 ),但我们仍然可以做到这一点。因此，我们可以安装 [chrome-aws-lambda](https://github.com/alixaxel/chrome-aws-lambda) 和[puppeter-core](https://www.npmjs.com/package/puppeteer-core)来代替木偶师，这是与木偶师相同的 API，只是没有默认下载 Chromium。将目录从工作文件夹切换到子文件夹 auto-web。然后 npm 安装 [chrome-aws-lambda](https://github.com/alixaxel/chrome-aws-lambda) 和[木偶核心](https://www.npmjs.com/package/puppeteer-core)

```
cd auto-web
npm install chrome-aws-lambda puppeteer-core
```

现在有了可调试和可部署的解决方案，是时候进入一些代码了。木偶师的文档非常好，所以如果你需要任何帮助，你可能会在 https://devdocs.io/puppeteer/找到你需要的东西。

由于 Puppeteer 只不过是 DevTools 协议上的一个高级 api，所以我们需要一个运行 Chromium 浏览器的实例。一旦浏览器启动并运行，Puppeteer 就可以建立到其主机的 Web 套接字连接，并开始交换消息。在 app.js 文件中:

```
const chromium = require('chrome-aws-lambda')
let browser = null
let page = null....
....
....exports.lambdaHandler = async (event, context) => {
try {
        browser = await chromium.puppeteer.launch({
            args: chromium.args,
            defaultViewport: chromium.defaultViewport,
            executablePath: await chromium.executablePath,
            headless: chromium.headless
        })....
....
....
```

chromium.puppeteer.launch 函数向浏览器对象返回一个承诺。它基本上启动浏览器，并通过 websockets 将 puppeteer 连接到正在运行的实例。有了浏览器对象，我们现在可以从浏览器创建一个页面对象。

```
page = await browser.newPage()
```

page 对象让我们可以访问我们需要的功能，比如浏览到一个 url 或者从页面上生成的 html 中获取一个选择器。我们现在可以使用页面对象与浏览器进行交互。

```
const navigationPromise = page.waitForNavigation()
await page.goto("https://www.example.com", { waitUntil: 'networkidle2' })
await page.setViewport({ width: 1920, height: 1001 })
await navigationPromise
```

在这里的第一行，我们使用了 [waitForNavigation](https://devdocs.io/puppeteer/index#pagewaitfornavigationoptions) 函数，它向主资源响应返回一个承诺。当页面导航到一个新的 URL 或重新加载时，这个承诺就兑现了。当我们想要阻止执行以确保页面在我们继续之前被加载时，这是很有用的。

下一行是 [goto](https://devdocs.io/puppeteer/index#pagegotourl-options) function 这将导航到指定的 url，在本例中为“https://www.example.com”。具有 waitUntil 属性的对象基本上是告诉 page 何时认为导航成功。在这种情况下，我们认为当至少`500`毫秒内没有超过 2 个网络连接时，导航就完成了。结果是一个与 [waitForNavigation](https://devdocs.io/puppeteer/index#pagewaitfornavigationoptions) 非常相似的承诺，它允许我们在页面完成导航之前阻止执行。

[setViewport](https://devdocs.io/puppeteer/index#pagesetviewportviewport) 调用只是设置视窗页面的预期宽度和高度。在此之后，我们阻止等待 navigationPromise，它将双重确保我们已经成功导航到我们正在寻找的页面。

现在我们在页面上，我们可以做一些屏幕报废，并从网页上获取一些值。

```
const footerleft = await page.waitForSelector('body > div > p:nth-child(2)')const textContent = await (await footerleft.getProperty('textContent')).jsonValue()
```

page 对象有一个非常有用的函数 [waitForSelector](https://devdocs.io/puppeteer/index#pagewaitforselectorselector-options) 。这个函数允许您指定一个[选择器](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Selectors)字符串(比如 jquery)并向选择器指定的[元素句柄](https://devdocs.io/puppeteer/index#class-elementhandle)返回一个承诺。顾名思义，它会等待选择器出现在页面上。不过，你需要小心，如果你试图获取的元素不在页面上，这只会在 30 秒后超时，默认情况下，这是 30 秒的 Lambda 时间，可能会变得很昂贵。

在这一点上，我们有一个 [ElementHandle](https://devdocs.io/puppeteer/index#class-elementhandle) 来从中获取任何属性值，我们需要使用 [getProperty](https://devdocs.io/puppeteer/index#elementhandlegetpropertypropertyname) 函数，它返回一个对 [JSHandle](https://devdocs.io/puppeteer/index#class-jshandle) 的承诺。不完全是我们所追求的字符串值，所以我们需要再做一个技巧，那就是 [jsonValue](https://devdocs.io/puppeteer/index#jshandlejsonvalue) ，它会给我们一个我们所追求的字符串值的承诺。

我们得到了现在可以浏览和屏幕抓取的代码。其他有用的功能是[页面。键入](https://devdocs.io/puppeteer/index#pagetypeselector-text-options)和[页面。单击](https://devdocs.io/puppeteer/index#pageclickselector-options)可以让您在文本框中键入文本或单击选择器指定的按钮。

确保您的函数可以优雅地退出总是一个好主意，所以确保您关闭页面并断开浏览器是一个好习惯。我们可以在 finally 块中这样做，以确保我们调用这些函数，不管我们可能遇到的任何异常。

```
....
....
....exports.lambdaHandler = async (event, context) => {
try {
....
....
....
    } catch (err) {
       console.log(err)
       return err
    } finally {
       if (page) {
          await page.close()
       }
       if (browser) {
          await browser.disconnect()
       }
}
```

代码应该如下所示:

```
const chromium = require('chrome-aws-lambda')let browser = null
let page = nullexports.lambdaHandler = async (event, context) => {
   try {
      browser = await chromium.puppeteer.launch({
         args: chromium.args,
         defaultViewport: chromium.defaultViewport,
         executablePath: await chromium.executablePath,
         headless: chromium.headless
      })

      page = await browser.newPage()
      const navigationPromise = page.waitForNavigation()
      await page.goto("https://www.example.com", { waitUntil: 'networkidle2' })
      await page.setViewport({ width: 1920, height: 1001 })
      await navigationPromise

      const footerleft = await page.waitForSelector('body > div > p:nth-child(2)')
      const textContent = await (await footerleft.getProperty('textContent')).jsonValue()
      return { textContent }
   } catch (err) {
      console.log(err)
      return err
   } finally {
      if (page) {
         await page.close()
      }
      if (browser) {
         await browser.disconnect()
      }
   }
}
```

此时，我们可以通过单步调试代码来验证一切正常。然而，我们仍然缺少的是一种在特定时间自动启动它的方法。这就是 cron 表达式发挥作用的地方，因此我们将重新访问 SAM 模板，并给出一个 cron [事件](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-events-rule.html)。最初，我在这里遇到一些困难，因为常规的 [cron 表达式](https://www.baeldung.com/cron-expressions)包括 6 个必填字段，第七年是可选的。 [AWS cron expression](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/ScheduledEvents.html) 但是似乎不包括秒，所以我们需要记住这一点。让我们将 Events 部分添加到 template.yaml 中的 AutoWebFunction 中。

```
Resources:
   AutoWebFunction:
   Type: AWS::Serverless::Function
   Properties:
      CodeUri: auto-web/
      Handler: app.lambdaHandler
      Runtime: nodejs8.10
      Events:
         CronEvent:
         Type: Schedule
         Properties:
            Schedule:
               cron(0 17 ? * FRI *)
```

这里的 cron 表情基本上每周五下午 5 点左右就会火。幸运的是，SAM 模板是 CloudFormation 模板的扩展，因此如果需要，我们可以轻松地包含新的资源以及添加参数，我更希望 cron 计划是可以作为参数指定的东西。因此，让我们添加一个新参数，我们可以用它来指定 cron 调度，而不是硬编码它。就在资源部分的上方，我们可以添加一个参数部分，它允许我们在 CloudFormation 开始提供堆栈时指定值。

```
Parameters:
   CronExpression:
      Type: String
      Default: cron(0 17 ? * FRI *)Resources:
   AutoWebFunction:
   Type: AWS::Serverless::Function
   Properties:
      CodeUri: auto-web
      Handler: app.lambdaHandler
      Runtime: nodejs8.10
      Events:
         CronEvent:
         Type: Schedule
         Properties:
            Schedule: Ref: CronExpression
```

现在最后一部分是 secrets，这意味着是时候添加 SecretsManager 资源了。正如我前面提到的，SAM 模板实际上是 CloudFormation 模板的扩展，因此添加 SecretsManager 资源的方式与在 CloudFormation 中的方式相同。SecretsManager 有一个将数据保存为 JSON 对象的惯例，所以如果我们要保存用户名和密码，我们应该使用 JSON 字符串来这样做。所以我们可能需要一个类似。

```
SecretString:
   '{"username":"user","password":"secret"}'
```

我认为用户名和密码也不是我想在模板中固定的东西，所以我更愿意把它们作为参数。

```
Parameters:
   CronExpression:
      Type: String
      Default: cron(0 17 ? * FRI *)
   Password:
      NoEcho: true
      Type: String
      Default: password
   UserName:
      NoEcho: true
      Type: String
      Default: username
```

在参数中，我使用了 [NoEcho: true](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/parameters-section-structure.html) 它的作用是让 AWS 知道，当提示输入值给模板时，它应该用星号(*)屏蔽这些值。掩盖密码之类的秘密是很有用的。

要添加 SecretsManager 资源，现在我们可以在 AutoWebFunction 下添加另一个资源。我们将把它命名为“autowebsecrets ”,这在以后会很重要。

```
Resources:
# the AutoWebFunction goes here
   AutoWebSecrets:
      Type: AWS::SecretsManager::Secret
      Properties:
         Name: autowebsecrets
         SecretString:
            Fn::Join:
            - ''
            - - '{"username":"'
              - Ref: UserName
              - '","password":"'
              - Ref: Password
              - '"}'
```

在模板中，我们使用了云形成函数 [Fn::Join](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-join.html) 。这是一个 CloudFormation 函数，允许我们将一组值附加到一个值中，由指定的分隔符分隔。如果分隔符是空字符串，则值集在没有分隔符的情况下连接在一起。在这种情况下，我们只是用它来做简单的字符串连接，以便为 SecretString 形成有效的 JSON 值。

然而，需要注意的一件重要事情是，运行该函数的角色需要读取机密的权限。幸运的是，在 AWS SAM 中有专门为这类事情构建的[策略模板](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-policy-templates.html)，并且有一个专门用于此目的的[AWSSecretsManagerGetSecretValuePolicy](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-policy-template-list.html#secrets-manager-get-secret-value-policy)。我们现在需要更改 AutoWebFunction 的资源声明，以包含此策略声明。

```
Resources:
   AutoWebFunction:
      Type: AWS::Serverless::Function
      Properties:
         Policies:
            - AWSSecretsManagerGetSecretValuePolicy:
                SecretArn:
                  !Ref AutoWebSecrets
         CodeUri: auto-web
         Handler: app.lambdaHandler
         Runtime: nodejs8.10
         Events:
            CronEvent:
               Type: Schedule
               Properties:
                  Schedule:
                     Ref: CronExpression
   AutoWebSecrets:
      Type: AWS::SecretsManager::Secret
      Properties:
         Name: autowebsecrets
         SecretString:
            Fn::Join:
            - ''
            - - '{"username":"'
              - Ref: UserName
              - '","password":"'
              - Ref: Password
              - '"}'
```

模板提供了秘密并授予角色足够的权限来读取秘密，现在我们可以将集成编码到 AWS SecretsManager 中。要做 se，我们需要添加 [aws-sdk-js](https://github.com/aws/aws-sdk-js) 库。所以回到工作目录中的 auto-web 子文件夹，安装 [aws-sdk](https://github.com/aws/aws-sdk-js) npm 库。

```
cd auto-web
npm install aws-sdk 
```

aws-sdk 让我们可以访问 aws 中可能需要的大部分资源，包括 SecretsManager。要访问 SecretsManager，我们必须首先创建一个客户端。一旦我们有了客户端，我们就可以使用我们之前在模板中指定的名称值作为 SecretId，在我们的例子中是“autowebsecrets”。需要注意的是 AWS。SecretsManager 需要一个区域请确保该区域是您要部署 SAM 模板的区域，因为这是您将用来设置资源的区域。在。/auto-web/app.js 文件:

```
....
const AWS = require('aws-sdk')
const client = new AWS.SecretsManager({ region: 'eu-west-1' })
....
....
....
```

客户端对象有 [getSecretValue](https://docs.aws.amazon.com/secretsmanager/latest/apireference/API_GetSecretValue.html) 函数，我们可以用它从秘密管理器中获取秘密。这个函数不返回一个承诺，而是将回调函数作为第二个参数，当调用完成时，您可以期望调用这个回调函数。回调函数接受两个参数，第一个是调用失败时的错误对象，第二个是包含重要属性 SecretString 的数据对象，这是一个字符串形式的解密 JSON 值，就像我们在模板中指定的一样。鉴于使用回调函数的尴尬，我决定创建一个助手函数，允许我通过使用 Promise 对象继续使用 await async。

```
function getSecret(secretId) {
   return new Promise((resolve, reject) => {
      client.getSecretValue({ SecretId: secretId }, (err, data) => {
         if (err) {
            reject(err)
            return
         }
         try {
            let result = JSON.parse(data.SecretString)
            resolve(result)
         } catch (e) {
            reject(e)
         }
      })
   })
};
```

基本上，这里发生的事情是，这个函数返回一个 Promise 对象，它允许我在一个异步函数中调用 await，如果回调出错或者抛出异常，我们会拒绝这个函数，否则我们会从 data 和 JSON 中获取 SecretString。解析(因为 SecretString 将采用 JSON 格式)。使用解析后的对象，我们将其作为承诺的解析返回。这允许我在异步函数中等待结果。

```
....
....
....
exports.lambdaHandler = async (event, context) => {
try {
   const autowebsecrets = await getSecret('autowebsecrets')
   const username = autowebsecrets.username
   const password = autowebsecrets.password
....
....
....
```

正如我前面提到的，中的 secretId 需要是我们在模板中提供的 SecretsManager 秘密的名称，在我们的例子中是“autowebsecrets”。最终的脚本应该是这样的:

```
const chromium = require('chrome-aws-lambda')
const AWS = require('aws-sdk')
const client = new AWS.SecretsManager({ region: 'eu-west-1' })let browser = null
let page = nullfunction getSecret(secretId) {
   return new Promise((resolve, reject) => {
      client.getSecretValue({ SecretId: secretId }, (err, data) => {
         if (err) {
            reject(err)
            return
         }
         try {
            let result = JSON.parse(data.SecretString)
            resolve(result)
         } catch (e) {
            reject(e)
         }
      })
   })
};exports.lambdaHandler = async (event, context) => {
   try {
      const autowebsecrets = await getSecret('autowebsecrets')
      const username = autowebsecrets.username
      const password = autowebsecrets.password

      browser = await chromium.puppeteer.launch({
         args: chromium.args,
         defaultViewport: chromium.defaultViewport,
         executablePath: await chromium.executablePath,
         headless: chromium.headless
      })
      page = await browser.newPage()

      const navigationPromise = page.waitForNavigation()
      await page.goto("https://www.example.com", { waitUntil: 'networkidle2' })

      await page.setViewport({ width: 1920, height: 1001 })

      await navigationPromise

      const footerleft = await page.waitForSelector('body > div > p:nth-child(2)')
      const textContent = await (await footerleft.getProperty('textContent')).jsonValue()

      return { username, password, textContent } } catch (err) {
      console.log(err)
      return err
   } finally {
      if (page) {
         await page.close()
      }
      if (browser) {
         await browser.disconnect()
      }
   }
}
```

经过所有这些艰苦的工作，我们终于可以部署了。在我们部署之前，需要注意的是，即使我们使用的是 [chrome-aws-lambda](https://github.com/alixaxel/chrome-aws-lambda) 浏览器，运行一个简单的示例页面也很容易占用 430 MB 的运行内存。因此，如果我们希望我们的函数正常工作，我们就必须为 lambda 函数的运行提供足够的内存。这就是为什么我们在模板的[全局变量](https://github.com/awslabs/serverless-application-model/blob/master/versions/2016-10-31.md#globals-section)部分添加了一个内存大小。当我们在[全局](https://github.com/awslabs/serverless-application-model/blob/master/versions/2016-10-31.md#globals-section)部分时，我还会将超时增加到 900 秒，这是可以给出的最高值，因为 lambda 的最大超时是 15 分钟。这应该给我们足够的时间来运行该功能，以防网页由于任何原因而变慢。

```
Globals:
   Function:
      Timeout: 900
      MemorySize: 512
```

为了进行部署，我们需要首先创建 S3 桶。我们需要一个“S3 bucket ”,在部署任何东西之前，我们可以上传打包为 ZIP 的 Lambda 函数。Lambda 基本上会将包提取到一个容器中，然后执行模板中指定的函数。因此，使用 AWS cli:

```
aws s3 mb s3://BUCKET_NAME
```

其中，存储桶名称是您帐户的唯一存储桶名称。为了打包解决方案，我们使用 sam package 命令。

```
sam package --template-file template.yaml --s3-bucket BUCKET_NAME --output-template-file template-out.yaml
```

同样，BUCKET_NAME 是您帐户的唯一存储桶。sam package 命令基本上压缩了您在 CodeUri 文件夹中为模板中的函数指定的文件夹的内容，它为该压缩文件指定了一个唯一的名称，并将其上传到 S3。如果您仔细查看 template-out.yaml 文件，您会注意到 CodeUri 的值与 template.yaml 中的值不同，它现在有一个有效的 S3://BUCKET _ NAME/random number URL。sam package 命令足够智能，可以自动计算出该属性的正确值，确保您的模板有正确的值供 CloudFormation 使用。

随着代码的上传和模板的更新，我们可以将应用程序部署到 AWS CloudFormation。要使用的模板文件应该是从 sam package 命令输出的文件(template-out.yaml)。栈名可以是您喜欢的任何名称，只要它符合栈名约定。您的区域需要是您希望部署堆栈的特定区域，如果您还记得之前在 SecretsManager 代码中指定了一个区域，请确保该值是相同的值，否则您的代码将无法工作。capabilities 是 AWS CloudFormation 创建特定堆栈之前必须指定的功能列表。由于该模板将代表您提供 IAM 角色，这意味着它将影响您的帐户的安全性，因此您需要指定 CAPABILITY_IAM 来基本上给予它这样做的权限。

```
sam deploy --template-file template-out.yaml --stack-name auto-web --capabilities CAPABILITY_IAM --region eu-west-1
```

现在我们的功能已经完成，应该按照 cron 计划执行了。如果您需要更改参数值，您可以简单地更改 CloudFormation 堆栈并再次启动配置。如果你需要测试，你可以转到 Lambda 部分并启动一个函数，它应该使用你在模板中指定的默认值。

# 结论

当我第一次着手这项工作时，我认为这将是一件简单而直接的事情。很快我发现了我未知的未知。对我来说，这强调了真正投入和行动的重要一课，直到那时你才真正知道所涉及的努力。这里有一个链接，链接到我在 Github 中的代码。