# 使用 Python + AWS Kinesis 的实时数据流—如何…(第 1 部分)

> 原文：<https://medium.com/swlh/real-time-data-streaming-with-python-aws-kinesis-how-to-part-1-cd56feb6fd0f>

在本文中，我们将重点介绍如何使用 AWS Kinesis 和 Python 以及 Node.js 将数据近乎实时地传输到 ElasticSearch。

Kinesis 为高通量数据处理和分析提供了基础设施。我们可以利用这一点将数据推送到存储/可视化服务，以进行聚合和历史查询。

在本指南中，我们将使用 Python 3.6 和 AWS 的 boto3、pandas 和内置函数。