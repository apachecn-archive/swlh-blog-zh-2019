# 编译时计数到无穷大

> 原文：<https://medium.com/swlh/counting-to-infinity-at-compile-time-2bbdd6446329>

## 越高越好越快越长

![](img/029b66c1b35688e22a2c3759a02c633a.png)

我们在我的[以前的博客文章](/@jdrphillips/counting-at-compile-time-6338aa471492)中看到了 scala 编译时计数的介绍，使用传统的`Nat`类型。

可惜，`Nat`慢了。真的，*真的*慢，而且我们能让 scala 编译器做的事情有严格的限制，就像它现在的样子。