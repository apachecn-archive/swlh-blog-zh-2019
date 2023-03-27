# NLB 连接重置

> 原文：<https://medium.com/swlh/nlb-connection-resets-109720accfc6>

## 使用/消费 AWS 的 NLB 或 NAT 网关？有些事你应该知道。

我对我们的下一代平台进行了负载测试，以使我们的性能与现有平台相当，并在非常低的负载下不断进行连接重置。

说明一下，我没发现 bug。这可能是 AWS“有意”关闭的问题之一。

# 设置

负载生成器是 VPC 中的一个 Locust.io 代理(我只用一个代理就能重现连接重置)