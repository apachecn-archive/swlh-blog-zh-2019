# Web 服务器:在没有应用程序的情况下提供私有文件

> 原文：<https://medium.com/swlh/web-server-serving-private-files-without-booting-the-application-63144f1175ed>

## 每次用户想要下载文件时停止启动应用程序的时间。

![](img/00b70264a7a97828f8ac7f82d3f5117c.png)

Photo by [chris panas](https://unsplash.com/@chrispanas?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

不久前，我接到一个任务，要找到一种方法将*私人*文件推送给用户，这种东西只有经过认证的用户才能看到。这通常是一个问题而不是一个特性:这个*私有*文件不应该被允许…