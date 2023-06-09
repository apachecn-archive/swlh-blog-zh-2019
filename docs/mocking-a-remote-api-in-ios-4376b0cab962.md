# 模仿 iOS 中的远程 API

> 原文：<https://medium.com/swlh/mocking-a-remote-api-in-ios-4376b0cab962>

## 基于面向协议的设计，使用 Swift

![](img/ed7fb665f3be573de01b052db7d96621.png)

Photo by [Sharon McCutcheon](https://unsplash.com/@sharonmccutcheon?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Swift 中的一项非常简单的技术利用了协议的优势，允许您几乎毫不费力地模仿应用程序的外部 API。在你目前的任务中，你肯定在做一个项目，这个项目意味着与外部服务器通信，而外部服务器将数据输入到你的 iOS 应用程序中。对这种外部资源的依赖创造了一个实现模拟对应物的机会，因为我们实际上可以将大部分开发时间用于模拟 API，而不是真正的 API。如今，我倾向于创建一个内部的模拟 API 对象，驻留在我的应用程序中，这样我就可以在开发应用程序 *的同时让**完全控制这种依赖关系。***这给了我一些明显的优势:

*   如果 API 不可用，我可以继续工作，因为我的模拟 API 是一个保证 100%正常运行的系统
*   我不再依赖网络层的速度。实际上，我设计了模拟 API，这样我就可以使用时间延迟来伪造一个异步请求，但是，除非我想知道类似于*UIActivityIndicatorView***在特定条件下如何表现之类的事情，否则我可以简单地摆脱延迟并加速我的工作。另一方面，如果我需要查看应用程序在低速互联网连接下的工作情况，我可以很容易地假装更长的延迟，而不需要使用任何黑客来抑制我的互联网连接**
*   **我可以毫不费力地随心所欲地操纵数据。即使我有一个开发环境(即，在我自己的机器上运行服务器)或访问远程登台服务器，如果我只需要在后端进行细微的更改来*测试 iOS 前端如何对该数据做出反应*，我需要或者进入某个 web 界面来更改该数据，或者，如果这甚至不存在，我需要直接进入数据库来进行更改(最终，我甚至可以在响应到达应用程序之前使用 Charles 这样的代理来编辑响应)。无论如何，所有这些方法都涉及几个步骤，可能需要一些时间来实现，即使我只想改变我从 API 得到的 *json* 中的一个字段。现在，如果我有一个模拟 API 对象，用本地 *json* 文件中的数据进行响应，会怎么样呢？这使得*数据操作像编辑单个文件一样简单，可以从 XCode* 访问**
*   **因为我拥有这个系统，所以我可以跳过任何依赖于服务器的资源。例如，想象一个每次启动时都显示登录屏幕的应用程序，而不是担心输入正确的用户名和密码来成功验证给定用户，为什么不简单地让模拟 API 成功响应而不管输入？毕竟，这个认证步骤不是我目前正在开发的功能的重点，所以我只想在模拟器中启动应用程序时克服它**
*   **当编写单元测试的时候，我可以免费得到所有的东西(我们都做单元测试，不是吗？)**

> **我从这一切中获得了什么？我在工作流程中节省了时间，这种加速应该足以激励我实现一个位于 iOS 应用程序“内部”的模拟 API。**

**模仿一个 API 就像在你的代码库中做一个小小的改变一样简单。无论你在你的应用中使用什么架构(MVC、MVVM、VIPER 等等)，只要你遵循基本的软件工程原则，有一个清晰的**关注点分离** *(* 通过一个封装了你所有 API 职责的类)你所需要做的就是重构你的代码，这样你就不再依赖于那个 API 具体的类类型，而是，你朝着一个协议定义的未知类型发展。**

# **作为协议的 API 层**

**根据你的应用程序的架构，你肯定有某种程度上隔离的网络层:无论它是一个包含所有内容的类，一个包含对其他 API 相关对象的引用的简单结构，某种围绕 *Alamofire* 的包装器，甚至是你自己的 HTTP 客户端，你都可能有它作为一个独立的对象，对你的 *UIViewControllers* 隐藏网络细节(说到这里，让 *UIViewController* 知道 API 层并不是一个好主意，但是现在，不使用类或结构，您可以使用协议，如下所示:**

**然后，您之前拥有的实际具体类型将简单地*符合这个协议*并实现所需的方法，就像它之前所做的那样，封装所有必要的逻辑以达到实际的 API。类似地，模拟 API 对象也将符合该协议，但是特定的实现将只生成模拟的数据:**

**这种重构的主要思想是**你的代码库中任何曾经调用具体 API 类的类都不应该再这样做，相反，应该只知道有一个类型符合 API 协议**。有了这个改变，你可以调用 API 协议上的任何方法，不管具体的实现是真正的 API 还是真正的模拟 API。**

> **凭借简单的面向协议的思维模式，我们刚刚赋予了我们的应用程序绕过所有网络呼叫的能力，这将被我们拥有和控制的模拟数据所取代。**

**至于如何以及何时决定使用哪个具体的实现，我选择了一种*运行时方法*,在这种方法中，根据应用程序启动期间传入的给定启动参数的存在，应用程序将使用一个或另一个实现。想象一下，我正在我的 *UIViewController* 中实例化我的 API 对象(这从来都不是一个好主意……但是，让我们再次把它排除在这个讨论之外)，我可以简单地这样做:**

**我喜欢使用命令行参数，这样我可以简单地创建一个新的*方案*，并在应用程序启动时传递给定的参数。由于我不会使用这种方案将应用程序归档到 App Store，我可以放心，默认情况下，我的代码库将针对真正的 API，但是出于开发目的，使用这种使用模拟版本的特殊*方案*非常方便。**

# **嘲笑数据**

**当谈到模仿实际数据时，有几种选择。一方面，我们可以简单地创建我们的结构、结构数组或闭包作为参数的任何类型，并简单地在方法的实现中用该参数调用闭包。另一方面，我们可以更进一步，加入一个从本地 *json* 文件中读取数据的实用程序，并将其解析为同一个闭包所接受的所需类型。我实际上更喜欢这种方式，因为我可以简单地将我期望的来自 *real* API 的任何响应放到一个单独的 *json* 文件中，并随意修改这些值。如果数据结构有点复杂，有许多嵌套的键值对，这也更容易阅读。**

**重要的是要记住，无论你选择哪种途径来模仿你的数据，你已经有了它的数据结构，因为它和你在真正的 API 实现中使用的是一样的。无论您使用的是符合 *Codable* 的结构，还是使用外部库中的其他东西，您在 API 协议的一个实现中用来保存数据的任何东西都与您在另一个实现中使用的数据结构完全相同。我们喜欢保持东西干燥，不是吗？**

# **添加一些味道**

**这个模拟 API 很酷的一点是，添加一些调整非常简单，在开发过程中可能会非常有帮助。你的网速是不是太快了以至于你不能鹰眼你是如何实现你的*ui activity indicator view*？好吧，让我们模拟一下 5 秒钟的延迟，这样我们就有足够的时间来看看当应用程序执行网络请求时我们的 UI 是什么样子:**

**这个简单的实现只是对 MockAPI 对象上实现的每个请求应用相同的延迟。90%的时间我都是零秒，因为正如我在开始时所说的，我只是想加快我的工作流程，我不希望 *UIActivityIndicatorViews* 出现在点击的时候。但是你可以随意发挥你的创造力，而不是在源代码中硬编码，为什么不在你的开发版本中添加一个*设置包*，这样你或者一个不太懂技术的团队成员就可以从设置应用中设置这个值了？**

**事情失败了怎么办？当服务器返回错误时，应用程序的外观和行为是否符合预期？特定的错误是否被正确处理？也许在一个给定的用例中，您想要处理不同的场景，并且您想要模拟其中的每一个场景。同样，当使用我们的模拟 API 时，没什么大不了的:**

**对于这个特定的请求，我们的应用程序的状态可以清楚地假设两种不同的扭曲，我们只需浏览它们，并检查我们的应用程序在这种情况下的行为。请记住，你没有黑任何东西，你实际上是在模仿一个非常正常的真实世界场景，无论出于什么原因，第一个请求失败了，你的应用程序应该表现良好，并准备尝试新的请求，然后会成功。当您使用远程 API 时，即使在测试环境中，这些边缘情况也不容易测试。**

# **最后的想法**

**我在 [Github](https://github.com/jpcarreira/MockAPI) 上有一个示例项目，演示了这里讨论的一些要点。这是一个非常简单的应用程序，它连接到[星球大战 API](https://swapi.co/) ，或者，如果在正确的方案下运行，将绕过它，使用模拟 API 实现。在这种情况下，它将根据应用程序附带的 *json* 文件显示外星人的电影。请忽略架构或超级简单的用户界面，这只是一个示例应用程序，重点是在这里讨论的主题。**

**在 iOS 应用程序中实现模拟 API 是一项非常简单的任务。我在这篇文章中的想法并不是指导你如何去做，即使是刚接触 iOS 和 Swift 的人，一旦他精通利用像这样简单的面向协议的编程技术，也能很容易地想到同样的概念。最重要的是，我想强调当我开始在我自己的项目上模仿远程 API 时我获得的*价值:我可以忘记后端，我不在乎后端团队是否实现了给定的端点，我不需要担心了解和访问后端堆栈来操纵数据，因为我可以直接在我的应用程序中这样做。让我再次强调这一点:现在几乎没有一个 iOS 应用程序没有后端服务器将数据输入到移动应用程序中，并能够完全独立于它的存在来构建它，这是一个将加快您的工作流程的好处。***

**然而，这种方法有一个很大的警告标志:*前端和后端之间的契约不能被破坏，*否则你的应用程序可能会崩溃或以非常意外的方式运行。只要与后端团队保持密切沟通，如果他们改变了他们同意发送回客户端应用程序的响应，请确保您及时了解这些变化，因为如果您太习惯于一直使用模拟 API 进行开发，那么很容易*忘记*当应用程序连接到实际的 API 时，事情可能不会像预期的那样工作。**

**我希望您了解了实现模拟 API 的好处，如果您正在将它应用到您当前的项目中，请让我们知道它是如何为您工作的！**