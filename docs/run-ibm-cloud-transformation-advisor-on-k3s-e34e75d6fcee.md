# 在 K3s 上运行 IBM 云转型顾问

> 原文：<https://medium.com/swlh/run-ibm-cloud-transformation-advisor-on-k3s-e34e75d6fcee>

![](img/715895015c53a4bedf6c49c9f7b7673b.png)

[IBM Cloud Transformation Advisor](https://www.ibm.com/cloud/garage/practices/learn/ibm-transformation-advisor)访问、分析传统的 J2EE 应用程序堆栈，建议迁移到云所需的更改/更新。它可以在 IBM Cloud Private 上运行，也可以通过 Docker 和 Docker compose 在本地运行。

在这里，我探索了在 K3s 上运行它的第三个选项。

## 准备舵图