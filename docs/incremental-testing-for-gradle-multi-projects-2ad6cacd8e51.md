# 梯度多项目的增量测试

> 原文：<https://medium.com/swlh/incremental-testing-for-gradle-multi-projects-2ad6cacd8e51>

![](img/40188c268cb8acb1284a53f1217b8975.png)

应用模块化在 Android 上相当普遍，甚至谷歌在上一期的 I/O 中也谈到了它。无论您只是使用它来分隔不同的领域，还是实现一个具有多个层次的清晰架构，您最终都会得到多个梯度子项目。模块化应用的好处之一是加快构建时间，但你可以走得更远。