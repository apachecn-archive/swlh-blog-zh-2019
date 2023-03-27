# 与纠缠的深度平等

> 原文：<https://medium.com/swlh/deep-equality-with-pester-a9a00c3cd8a1>

## 应用递归和类型来填补 Pester 最大的特性空白

*如果你还没有，学习一下* [*【纠缠】*](https://github.com/pester/Pester) *的基础知识，开始在你的代码中添加一些单元测试。*

[纠缠](https://github.com/pester/Pester)是*在 PowerShell 中编写单元测试的*框架。然而，纠缠有一个大问题…

> PowerShell 完全是关于数组的，然而 Pester 的`Should -Be`并不适用于数组或任何其他复杂类型。