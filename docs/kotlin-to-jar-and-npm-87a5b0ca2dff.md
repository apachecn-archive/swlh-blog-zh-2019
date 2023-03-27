# 用 Kotlin 多平台创建一个编译成 NPM 和 Jar 的库

> 原文：<https://medium.com/swlh/kotlin-to-jar-and-npm-87a5b0ca2dff>

***构建 Kotlin 项目的教程，该项目编译成 NPM 包(用于 NodeJS 和浏览器)和 JVM Jar***

最近，我开始为我们的组织编写一个简单的单位转换库。这带来了一个有趣的难题:我们同时使用 Java 和 Javascript，重要的是转换在所有地方都是相同的。编写两个库并确保它们功能相同似乎不太容易维护，即使对于这样一个小项目也是如此。
但后来我想起来了:**科特林** …