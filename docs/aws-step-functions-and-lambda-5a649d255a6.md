# 使用 AWS 步骤函数和 Lambda 的无服务器编排工作流

> 原文：<https://medium.com/swlh/aws-step-functions-and-lambda-5a649d255a6>

![](img/daa0b513bc72c4ec4dfea4a62041cfa0.png)

Image by [Gerd Altmann](https://pixabay.com/users/geralt-9301/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=995567) from [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=995567)

AWS [**步骤功能**](https://docs.aws.amazon.com/step-functions/latest/dg/welcome.html) 服务允许使用 Lambda 功能设计和执行无服务器编排工作流。

首先，概述一下 AWS[**λ函数**](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html) :

AWS Lambda 是一种计算服务，让您无需配置或管理服务器即可运行代码。

> Lambda 提供对无服务器的支持…