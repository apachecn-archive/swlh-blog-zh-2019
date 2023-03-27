# 使用 API 网关和 Java Lambda 代理处理多部分/表单数据以便在 S3 存储

> 原文：<https://medium.com/swlh/processing-multipart-form-data-using-api-gateway-and-a-java-lambda-proxy-for-storage-in-s3-e6598033ff3e>

![](img/1d410a8028c75df30f6635a9c8e96262.png)

Photo by [JOSHUA COLEMAN](https://unsplash.com/@joshstyle?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我最近在为 Cork Hounds 做一个新项目，允许我们的追随者为我们新的每月摄影比赛提交他们的狗在狗友好酒厂的照片。我需要弄清楚如何将文件上传到亚马逊网络服务(AWS)的简单存储服务(S3 ),使用 API Gateway 作为用…编写的 Lambda 函数的 [Lambda 代理](https://docs.aws.amazon.com/apigateway/latest/developerguide/set-up-lambda-proxy-integrations.html)