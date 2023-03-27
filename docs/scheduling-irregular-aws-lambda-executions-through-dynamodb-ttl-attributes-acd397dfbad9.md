# 不规则调用的无服务器调度

> 原文：<https://medium.com/swlh/scheduling-irregular-aws-lambda-executions-through-dynamodb-ttl-attributes-acd397dfbad9>

![](img/7df26674683caf6295bd35e25cfadeac.png)

Photo by [Alexandre Chambon](https://unsplash.com/@goodspleen?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 介绍

本文描述了一种通过使用 AWS DynamoDB TTL 属性和流来调度 AWS Lambda 调用的无服务器方法。在撰写本文时，没有办法在不滥用 CloudWatch crons 的情况下安排 lambda 执行的不规则时间点(例如“在 2 天 10 小时内运行一次该函数，然后在 4 天内再次运行一次”)