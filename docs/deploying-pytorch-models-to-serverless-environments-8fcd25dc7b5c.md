# 将 Pytorch 模型部署到无服务器环境

> 原文：<https://medium.com/swlh/deploying-pytorch-models-to-serverless-environments-8fcd25dc7b5c>

谷歌云功能 vs Azure 功能 vs AWS Lambda

在本文中，让我们来看看将深度学习模型部署到生产中的一些最简单的方法。我使用 pytorch 内置的[虹膜](https://archive.ics.uci.edu/ml/datasets/iris)分类器。我不会讨论模型的实际构建或训练。我们将直接跳到部署部分。下面是构建、训练和保存 pytorch 模型以对鸢尾花进行分类的最小代码。