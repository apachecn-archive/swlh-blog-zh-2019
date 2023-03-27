# 在 Dotnet Core 中生成代码覆盖报告

> 原文：<https://medium.com/swlh/generating-code-coverage-reports-in-dotnet-core-bb0c8cca66>

像其他应用程序框架一样**。NET Core** 不提供开箱即用的代码覆盖报告，甚至代码覆盖[支持](https://github.com/microsoft/vstest/issues/981)也只在 dotnet core 版本中提供。但是'**代码覆盖率分析**是 Visual Studio 企业版提供的。你可以在这里找到更多信息[。](https://docs.microsoft.com/en-us/visualstudio/test/using-code-coverage-to-determine-how-much-code-is-being-tested?view=vs-2017)

没关系，我们有其他工具可以获得代码覆盖率报告，在这里我将简单解释一下。已经有一篇关于这个[话题](https://gunnarpeipman.com/aspnet/code-coverage/)的博文，作者是 [Gunnar Peipman](https://twitter.com/gpeipman) 。你也可以参考一下。在这里，我将使用不同的方法…