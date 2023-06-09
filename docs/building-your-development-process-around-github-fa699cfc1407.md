# 围绕 GitHub 构建您的开发流程

> 原文：<https://medium.com/swlh/building-your-development-process-around-github-fa699cfc1407>

![](img/ae91720c96a4bb30087a1f88cd4f9fe0.png)

GitHub 是一个基于网络的托管平台，你可以托管开源项目，并关注你喜欢并希望采用的项目。它使用 Git，这是一个分布式版本控制系统。

开发者喜欢 GitHub 平台。GitHub 不仅为他们提供了简化工作的工具，还为团队领导和项目经理提供了与开发人员联系、审查工作、跟踪进度和自动化工作流程所需的工具。

简而言之，GitHub 是一个优秀的工具，我建议您围绕它来构建开发流程。

# **关于 GitHub 的更多信息**

具体来说，GitHub 提供了访问控制和协作特性，帮助您管理与项目相关的 bug、特性和文档。使用 GitHub，您可以在一个与源代码紧密相关且为开发人员所熟悉的环境中执行几项与项目相关的任务，从而提高项目管理工作的效率。

就定价而言，GitHub 为公共存储库提供了免费计划。如果你喜欢创建无限数量的私有存储库，它还提供了一个灵活的付费计划。

# **什么是饭桶？**

Git 是免费和开源的，由 Linux 内核的创建者和主要开发者 Linus Torvalds 于 2005 年创建。Git 用于跟踪源代码文件中的变化，同时协调多人的工作。

在 GitHub 中，开发人员可以通过网络访问服务器上的 Git 存储库，这样他们就可以提交代码并进行协作。因为它是一个分布式系统，所以每个 Git 存储库都包含一整套文件和变更历史，并且可以独立于到其他存储库的网络连接而工作。结果是一个危险的系统，保持数据完整。

# **通过持续集成避免合并问题**

当开发人员开始一个新项目时，他们会制作当前代码库的副本，创建一个单独的分支，然后开始工作。随着开发人员的工作，其他开发人员不断更新代码库，添加新的库和依赖项。

因此，当开发人员试图将他们工作的副本与主代码库合并时，他们的副本不再反映主代码库，这导致了冲突。这就是所谓的*合并地狱*。开发人员在副本上工作的时间越长，问题就变得越“可怕”，因为他们的副本和主代码库之间的差异越大。

GitHub 帮助用户远离合并地狱，因为它利用了软件开发中的最佳实践之一:*持续集成*。

持续集成是合并开发人员编写的代码的实践，一天几次。目标是减少每次合并需要完成的工作量。如果工作需要很长时间才能完成，开发人员必须更频繁地更新他们的主代码库副本，以便合并其他人所做的增量更改。

持续集成最好与自动化单元测试一起使用，用于确认代码库在每次集成尝试后都按预期工作。当测试失败时，开发人员立即知道问题出在哪里，并可以快速修复它们。

# **使用拉取请求改善协作**

pull 请求是 GitHub 创建的一个独特功能。这是传统项目管理应用程序所不具备的强大协作工具。

它是这样工作的。通常，当开发人员开始开发一个特性时，他们会创建一个独立的代码库副本。这个副本叫做*分支*。每当开发人员想要将代码与主代码库(被称为*主分支*)合并时，他们会创建一个拉请求，以便可以审查代码。

拉请求允许团队领导和其他开发人员审查代码中所做的更改。然后，他们可以接受这些更改，也可以要求对某些部分进行返工。在 pull 请求中，您可以对单独的代码行进行注释，并提及开发人员以开始讨论。

# **使用问题功能更好地管理项目**

虽然拉请求对于协调和审查开发人员的工作很有用，但是 GitHub 还有另外一个小东西给项目经理——一个叫做*问题*的特性。Issues 用于跟踪 bug 或需要开发人员完成的大量工作。

使用该功能，您可以做很多事情。您可以标记问题并确定其优先级；将您创建的问题合并到里程碑中，以便跟踪您在发布过程中取得的总体进展；轻松地将问题分配给开发人员，以便明确谁对什么负责；为问题附加丰富的描述；和标签问题，以便轻松跟踪所有相关活动。

# **或者，使用项目功能更好地管理项目**

问题是好事。但是他们的默认列表视图很难使用，尤其是当你有很多问题的时候。

为了让项目经理的生活更轻松一点，GitHub 团队创建了一个名为 *Projects* 的特殊视图。项目是一个看板，由存储库中的问题、附注和拉式请求组成，在您定义的列中分类为卡片。这些卡片包含您可能需要的所有相关元数据——标签、受托人、状态等等。您可以轻松地拖放卡片，或者使用键盘快捷键来重新排列它们。

# **使用 GitHub Wiki 有效地记录项目**

使用 GitHub Wiki，您可以轻松地创建页面并将它们相互链接以创建项目文档。如果您的存储库是私有的，只有有权访问它的人才能看到 wiki 页面。

虽然与 Google Docs 相比，GitHub Wiki 有些局限，但它仍然是一个非常有用的工具，可以保存开发过程中需要的文档——环境设置指令、部署指令、各种模块的描述以及代码库的片段。当你接触新的开发人员时，这些文档将是无价的，并且应该一直保持更新。

# **结果**

GitHub 并不是唯一可以帮助你构建开发过程的工具。但在我看来，这是最容易把握的，也是最受欢迎的。所以如果你不是开发者，以前也没用过 GitHub，那就去看看吧。你可能需要一些时间来学习如何使用它，但我认为任何投入的时间最终都会得到回报。

[![](img/308a8d84fb9b2fab43d66c117fcc4bb4.png)](https://medium.com/swlh)

## 这篇文章发表在[《创业](https://medium.com/swlh)》上，这是 Medium 最大的创业刊物，有+420，678 人关注。

## 订阅接收[我们的头条新闻](https://growthsupply.com/the-startup-newsletter/)。

[![](img/b0164736ea17a63403e660de5dedf91a.png)](https://medium.com/swlh)