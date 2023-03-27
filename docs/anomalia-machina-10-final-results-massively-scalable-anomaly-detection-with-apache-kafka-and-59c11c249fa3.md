# Anomalia 玛奇纳 10 —最终结果:使用 Apache Kafka 和 Cassandra 进行大规模可扩展异常检测

> 原文：<https://medium.com/swlh/anomalia-machina-10-final-results-massively-scalable-anomaly-detection-with-apache-kafka-and-59c11c249fa3>

在 Anomalia 玛奇纳系列的第十篇也是最后一篇博客中，我们调整了异常检测系统，并成功地将应用程序从 3 个 Cassandra 节点扩展到 48 个 Cassandra 节点，并获得了一些令人印象深刻的数字:574 个 CPU 核心(跨 Cassandra、Kafka 和 Kubernetes 集群)，每秒 230 万次写入 Kafka(峰值)，每秒 220，000 次异常检查(可持续)，这相当于每天 190 亿次异常检查。

## 1.攀登之旅