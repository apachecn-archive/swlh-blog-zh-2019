# 在 Kubernetes 中使用 Prometheus 监控 Akka Streams Kafka (Alpakka)应用程序

> 原文：<https://medium.com/swlh/monitoring-akka-streams-kafka-alpakka-apps-with-prometheus-in-kubernetes-e1653c6fe6a0>

![](img/b7177cb90f27cca3fd791c7035f884a2.png)

## 从任何 Scala 或 Java 应用中抓取消费者和生产者指标

## TL；速度三角形定位法(dead reckoning)

这篇文章的重点是监测你的卡夫卡部署在库伯内特斯与普罗米修斯。Kafka 通过 JMX 公开其指标，对于使用其 Java SDK 的应用程序也是如此。能够拥有…