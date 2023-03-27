# 博尼尼悖论与数字技术

> 原文：<https://medium.com/swlh/boninis-paradox-and-digital-technology-4a414f535e02>

为什么 IT 组织不能总是让客户满意。

![](img/5ac5923202a3e4fe0b86b0a565a0f1d7.png)

Photo by [Thor Alvis](https://unsplash.com/@terminath0r?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) (modified by me)

我有一张美国地图...实际尺寸。上面写着，“比例尺:1 英里= 1 英里。”去年夏天我一直在折叠它。我几乎从不打开它。人们问我住在哪里，我说‘E6’。
——史蒂夫·赖特

赖特的笑话既有趣又深刻。当我在 IT 界的时候，我经常想起这个笑话。它很好地概括了 It 专业人员一直面临的挑战——我们如何开发、提供和支持足够复杂的系统来满足客户需求，同时又是可管理和可持续的。

这是一场让所有人都不满意的斗争。客户希望并应该能够自主地将现有技术用于最具创造性和相关性的目的:

*   *让我们来模拟一下这个系统是如何工作的！*
*   我认识一个能编写代码的人。
*   我刚刚读到这个巨大的分布式框架，它最终拥有了完成我们需要的工作的计算能力...

IT 专业人员希望并且应该受到约束，以便在一切顺利和出现问题时支持客户:

*   所以从来没有人安排过一次适当的备份？
*   *谁写的这段意大利面条代码？*
*   *抱歉，加密是真实的——重启无法修复勒索软件……*

![](img/4cad4a91fc0515e5d43ce7ffdce1edb9.png)

Photo by [John Barkiple](https://unsplash.com/@barkiple?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 复杂性把标准化当早餐吃

“博尼尼悖论是一个问题的名称，当一个现象的模型和它应该解释的现象一样难以理解时，这个问题就会出现。”
- 阿尔伯塔大学认知科学词典

我们给自己讲了一个关于技术的故事，一个随着时间的推移而演变的故事，它非常符合美国繁荣的承诺。它运行在连续的循环中，并由技术公司的市场营销进行提炼。这是一个关于数字技术是摆脱博尼尼悖论的出路的故事。

如果我们在人机交互上多应用一点资源，我们将使事情变得简单，任何人都可以使用它。如果我们认真倾听客户的需求，并忠实地将它们融入到产品中，对客户来说将是完美的。如果我们确保应用最好的编码原则，并小心地包含我们的层和过程，系统将更易于管理。

让我们来看看每一个，以了解他们是否解决了博尼尼悖论。

![](img/8951925c02d4a4c2634ea986db0869c7.png)

Photo by [Franck V.](https://unsplash.com/@franckinjapan?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 人机交互(HCI)

人机交互以奇迹般的方式推进了数字技术领域。一个蹒跚学步的孩子可以使用数以千计的应用程序进行娱乐和教育。不了解 LISP 和 Basic 区别的成年人可以在机场轻松地使用触摸屏打印登机牌。然而，用户体验的简单性是以开发和管理系统的巨大成本为代价的。一个 VAX 终端可能无法与你对话，但它可以由一组人合理地管理。Siri 背后有多少人、多少系统和多少行代码告诉你这个被零除的笑话？

我不认为人机交互是一种逃避悖论的方法；我认为它对用户掩盖了复杂性，达到了真正美妙的目的。面具很漂亮，也很有用，但并没有降低系统整体的复杂度。博尼尼仍然困住了我们。

![](img/e07617e464f161cfb8826065b09a7ae2.png)

Photo by [Daria Nepriakhina](https://unsplash.com/@epicantus?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 需求收集

从客户那里获得最好的需求是一门艺术。IT 领域已经得到了不倾听客户需求的惨痛教训。现在，最好的组织将流程融入到产品的整个生命周期中。大量的需求收集有助于设定产品的范围，同时设定用户的期望。显然，这打败了博尼尼，#阿米利特！

除了个人改变想法，组织进化。因此，我们必须确保构建的系统能够为未来的发展提供一定程度的灵活性。因此，我们开发了 bolt on 代码，用新模块扩展了系统，并偏离了传统的安装方式。

最终，该系统不再能满足新的要求。所以，我们投资了一个新系统。我们对全体员工进行新系统的培训。我们一次又一次地计划平稳的迁移，然后发现我们的不足之处。我们仍然感激博尼尼。

![](img/aab02654c096e46ab297a05d1a5b509b.png)

Photo by [frank mckenna](https://unsplash.com/@frankiefoto?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 模块化、模式和层级(天啊)

这里我指的是确保我们的系统相互包容的最佳实践，只有非常特定的接口暴露出来进行交互。就像换一个汽车音响一样。汽车音响被设计成可互换和可替换的(不用替换你的整辆车)。汽车中的大多数系统都是根据这一原则设计的。当它们没有得到很好的控制时，我们最终会收到来自机械师的巨额账单，因为他们不得不移除四个不同的制冷器来更换一个 gibblerod。

因此，它与良好的系统设计和最佳编码实践相结合。客户需要使用更新、更好的数据库应用程序。在理想的系统中，要做到这一点，只需要做一些改动。对于更简单的系统，只需几处改动就可以完成，可能需要五分钟！

但是，我们对系统的要求越多，系统为满足新要求而进行的有机进化越多，这个过程就变得越困难。

此外，对于 IT 人员来说，模块化是有成本的。新的控制方法需要新的培训。理想情况下，接口很容易被替换掉，但是最好的组织仍然会在各种质量控制方法上花费大量的时间和金钱。模块化和编码的标准和最佳实践也在发展，这需要个人重新培训和重构代码。

![](img/c74cf3b0fe67d92029221acd38cfec40.png)

Photo by [Jose Martinez](https://unsplash.com/@jmartinez143?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

三十多年后，保罗·西蒙仍然是正确的——这些是*创造奇迹和奇迹的日子*(尽管我的孩子们不理解长途电话的概念)。但与此同时，维德先生很久很久以前是正确的，他说，'*不要对你建造的这个科技恐怖太骄傲。*’

*我希望我有助于揭示这个问题，以及它如何造成组织和人际摩擦。我在其他领域看到了相似之处，比如信息检索，但是数字计算的普及使这个问题变得更加突出。*

*也许有其他思考数字技术的方法可以帮助我们解决博尼尼悖论？让我知道你的想法。*

*[1][http://www . bcp . psych . ualberta . ca/~ Mike/Pearl _ Street/Dictionary/contents/B/bonini . html](http://www.bcp.psych.ualberta.ca/~mike/Pearl_Street/Dictionary/contents/B/bonini.html)*

*[https://www.paulsimon.com/track/the-boy-in-the-bubble-6/](https://www.paulsimon.com/track/the-boy-in-the-bubble-6/)*

*[3][https://en . wiki quote . org/wiki/Star _ Wars _(film)](https://en.wikiquote.org/wiki/Star_Wars_(film))*