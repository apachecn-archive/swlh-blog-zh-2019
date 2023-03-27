# 用 Go 编写负载测试工具

> 原文：<https://medium.com/swlh/writing-a-load-testing-tool-in-go-8afac300d5ec>

![](img/899d89e492003ee3199b1a680653a439.png)

# **动机:**

在我之前的一个组织中，我们有一个基于 Python 和 Scala 的微服务的负载测试需求，这些微服务通过 RabbitMQ 进行交互(使用与 AMQP 1.0 非常不同的 AMQP 0.9 协议)

我们已经在使用 Jmeter 作为 Rest APIs 的首选负载测试工具。所以我没有多少选择