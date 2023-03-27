# 用 Maven 项目打包源代码

> 原文：<https://medium.com/swlh/packing-source-code-with-maven-project-90e7873bbc36>

![](img/77dc515e83cbce04857b7770eed90a0a.png)

开发人员经常需要调试和修改 Maven 项目的源代码，即使是在构建和部署了 **Jar/War** 文件之后。

我们可以通过以下两种方式获取源代码。

*   ***使用 Java 反编译器反编译 Jar/War 文件。***
*   ***将源代码打包成一个单独的 Jar/War 文件。***