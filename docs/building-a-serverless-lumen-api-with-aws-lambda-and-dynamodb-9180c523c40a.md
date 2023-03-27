# 用 AWS Lambda 和 DynamoDB 构建无服务器 Lumen API

> 原文：<https://medium.com/swlh/building-a-serverless-lumen-api-with-aws-lambda-and-dynamodb-9180c523c40a>

在我之前的[帖子](/@igliop/https-medium-com-igliop-running-a-serveless-lumen-rest-api-on-aws-lambda-804089b0852c)中，我解释了如何在 AWS 上使用自定义 PHP 运行时运行 Lumen Laravel serverless。在这篇文章中，我将展示如何在 Lumen Laravel 中构建一个无服务器的 RESTful API 来添加、删除、更新和检索存储在 Dynamodb 中的示例电影数据。

本文并不意味着是一个精确的分步指南:我只包括了达到目标的主要步骤。如果你认为我遗漏了一些基本的细节，请在下面评论。

# 体系结构