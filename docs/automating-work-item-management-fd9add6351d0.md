# 自动化工作项管理

> 原文：<https://medium.com/swlh/automating-work-item-management-fd9add6351d0>

## 使用 PowerShell 跟踪 TODOs

*第八部分 [*声明式 DevOps 微框架*](/@cjkuech/declarative-devops-microframeworks-9908c8d05332)*

*如果你已经开始了一个新的代码库，你可能会在你的代码注释中留下很多`TODO`来表示你需要在以后的某一天回到特定区域的实现上。这些`TODO`帮助你关注整个代码流，而不是被一个小的实现细节分散注意力。*

*你是如何跟踪这些`TODO`的？我们可以利用 PowerShell 的本地正则表达式和 I/O 功能来创建一个轻量级的…*