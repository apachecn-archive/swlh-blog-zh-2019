# 召回、精确度、F1、ROC、AUC 和一切

> 原文：<https://medium.com/swlh/recall-precision-f1-roc-auc-and-everything-542aedf322b9>

您的老板要求您构建一个欺诈检测分类器，因此您已经创建了一个。

![](img/eaa720adaea4d2cbb35bb0842d2f2f4c.png)

您的欺诈检测模型的输出是交易为*欺诈*的概率[0.0–1.0]。如果这个概率低于 **0.5** ，你就把交易归类为*非欺诈；否则*，你将交易归类为*欺诈*。