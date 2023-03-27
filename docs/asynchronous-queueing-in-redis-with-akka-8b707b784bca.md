# 基于 Akka 的 Redis 异步排队

> 原文：<https://medium.com/swlh/asynchronous-queueing-in-redis-with-akka-8b707b784bca>

![](img/de3be5665bd33c80a6e1f2ade2781755.png)

这篇文章假设读者对 Scala 和 Akka 有基本的了解。

我正在做一个项目，涉及通过第三方 API 发送短信，我需要检查消息是否发送，以通知用户消息状态的服务。