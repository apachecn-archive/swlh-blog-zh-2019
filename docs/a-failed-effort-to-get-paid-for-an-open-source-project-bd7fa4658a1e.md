# 为开源项目获得报酬的失败努力

> 原文：<https://medium.com/swlh/a-failed-effort-to-get-paid-for-an-open-source-project-bd7fa4658a1e>

![](img/525493943641c8fef07fee73b82e9de7.png)

[https://unsplash.com/photos/Tjbk79TARiE](https://unsplash.com/photos/Tjbk79TARiE)

在一家只有 4 个人的初创公司，我们的工程团队不得不走很多捷径来发布我们认为需要的不断变化的功能列表。我们在代码中加入了大量的`TODO`注释，希望能够组织越来越多的技术债务。元数据经过深思熟虑地添加到注释中，希望让事情有条理，但我们很快就失去了大部分的线索。我们有很多代码，没有很多人，构建和维护我们系统的开发工作流很快变得不那么愉快。

在业余时间，我为自己和我的团队开发了一个工具: [Toodles](https://github.com/aviaviavi/toodles) 。这是一个工具，它会扫描所有`TODO`条目的代码库，组织它们，并使您能够进行元数据更改，这些更改将直接应用于代码库的`TODO`条目。我认为这是一个很棒的工具！让我高兴的是，当我把代码放到 GitHub 上时，其他人似乎也是这么想的。人上星，分叉，克隆，下载。Toodles 开始收到来自世界各地的捐款，我惊讶地看着它的发展。

受到网上和同事们热情的鼓励，我在空闲时间继续制作《再见》。像大多数项目一样，GitHub 活动最终慢了下来，但并没有消失。我偶尔会收到错误报告，有时是来自那些似乎在更大的公司工作的人。我不想让我的软件看起来有问题，所以我一有时间就修复它，但是这个想法会冒出来:

*为什么我会志愿去一家大公司工作？为什么我连志愿服务的时间都给了* ***我的*** *用人公司*？

在这个问题上炖了一会儿，我得出的结论是，可以尝试对 Toodles 收费。毕竟，我在一个公司正在使用的工具上投入了大量的工作，对我来说，尝试获得报酬是公平的。更多的问题立即沸腾起来。

*   顾客在哪里可以买到图多？我该怎么给他们？
*   有人经常使用 Toodles 吗？因为它是在用户的机器上下载和运行的，而且我没有编写自定义代码来记录使用数据，所以我甚至不能确定这种努力是否值得我花费时间。
*   如果它确实有固定用户，他们中的任何人真的会为它付费吗？
*   如果他们愿意，我该如何向他们收取费用？
*   我怎么能向公司收取 Toodles 的费用，而对个人免费，而不为此做大量的编码工作呢？
*   在其他人参与的开源项目上赚钱，却不付钱给他们，这道德吗？我怎样才能给所有的贡献者寄钱，或者公平地决定付给他们多少钱？

我决定首先解决使用统计的问题。Toodles 是一个安装在用户机器上的程序，所以为了收集任何数据，它需要远程登录到一个我必须支持的端点。一些开发者对这个想法相当敌视(例如:[https://github.com/lihaoyi/Ammonite/issues/607](https://github.com/lihaoyi/Ammonite/issues/607))。我不能使用传统的基于云的日志服务，因为这需要在用户机器上安装一个日志收集守护程序。设置我自己的后端只是为了收集本地运行的应用程序的使用统计数据，这看起来也是多余的。如果这个问题有一个合适的解决方案，我仍然没有找到。

尽管缺乏数据和普遍的不确定性，我决定向前迈进，制作一个针对公司的付费层。为了将个人和公司分开，Toodles 扫描的`TODO`条目数量有一个上限，门槛设置得足够高，只有非常大的代码库才能触发。因为 Toodles 本身是一个用户在其机器上运行的服务器，认证一个付费用户是很棘手的。我有两个选择:

1.  建立或购买一个后端系统，该系统将跟踪用户帐户、购买等，并使 Toodles 客户端与之对话。
2.  分发 Toodles 可以验证的许可文件，以便访问付费功能。

构建选项(1)的工作量非常大，所以我想避免这种情况。选项(1)的付费服务似乎不存在。别无选择，我只能选择第二种。我不知道如何创建和验证一个许可文件，但经过一段令人沮丧的时间后，我终于明白了。我希望我的努力是值得的。

接下来，我必须支持一个在线商店，这样人们就可以购买许可文件。很难找到一个在线商店服务，可以让我在客户购买时以编程方式生成许可证文件。我最终在电子商务网站 fastspring.com 上找到了这个功能。设置起来很棘手。随着我努力的兔子洞越陷越深，投资回报似乎越来越不可能是正数。

在让 FastSpring 生成我的许可文件的过程中，我的网上商店开始工作了。它看起来像是 2000 年早期的网上商店，但至少它在工作。我设置了一个简单的登录页面，该页面将重定向到最终购买的商店。

随着网上商店的完成，我准备开始销售 Toodles Pro！我必须向 FastSpring 支付我所做的任何销售的 5%的费用，但至少我现在可以赚钱了。商店看起来不太好，因为 FastSpring 似乎不太注重设计，但总比没有好。这是功能性的，但由于所有的努力进入所有的设置，它留下了很多需要。

一切就绪后，我等着看是否有任何现有用户会升级到 Toodles 的付费层。过了一段时间后…

什么都没发生。迄今为止，没有人购买过 Toodles Pro。我意识到，如果我想有任何收入，我需要花更多的时间来销售这个项目。但我并不是想和图德尔斯一起创办一家成熟的公司；我只是想捕捉我的工作给商业实体带来的一些价值。很明显，赚钱需要大量的前期努力，这与我这个项目的真正目标无关。

所以我放弃了。为了将我的开源项目货币化，有太多的问题花费了或将要花费太多的时间:

*   建立一种收取软件费用的方式很困难。甚至首先弄清楚使用哪个平台都很困难。
*   因为支付解决方案不是项目的*发行版*的一部分，仅仅处理支付是不够的。现在，应用程序代码与我设法用 FastSpring 实现许可证生成紧密耦合。许可证验证和用户层处理必须直接嵌入代码中才能正常工作。像这样的关注点的紧密耦合牺牲了代码的可维护性和灵活性。很难改变一个而不返工另一个。
*   最重要的是，评估我是否甚至*应该*解决这些问题来赚钱我的项目是不清楚的，因为我没有关于人们是否/何时/如何使用我的工具的数据！我没有简单的方法找到答案，因为软件是安装在用户的机器上的。

不幸的事实是，我花在赚钱上的时间并没有花在让 Toodles 更好地为用户服务上。为了解决我们开源开发者的这个问题，我们需要一种在中内置支付和使用报告*的方式来分发我们的软件，这样开发者就可以专注于构建和管理用户喜欢的开源项目。*

当我继续解决这个问题时，我会发布更多关于这个问题的想法。继续关注更多内容。