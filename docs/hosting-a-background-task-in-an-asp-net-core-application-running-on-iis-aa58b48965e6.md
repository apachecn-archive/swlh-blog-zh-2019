# 在 IIS 上运行的 ASP.NET 核心应用程序中托管后台任务。

> 原文：<https://medium.com/swlh/hosting-a-background-task-in-an-asp-net-core-application-running-on-iis-aa58b48965e6>

在这篇文章中，我分享了我如何使用 ASP.NET 核心中可用的`Microsoft.Extensions.Hosting.IHostedService`接口和`System.Thread.Timer`类在指定的时间间隔运行后台作业。它在很大程度上是简单的，微软提供了关于这些库的很好的文档。文档中不清楚的一点是，当一个调用开始，而前一个调用在指定的时间间隔内没有完成时，如何处理重叠的调用。如果您在 IIS 上托管您的 ASP.NET 核心，请查看…