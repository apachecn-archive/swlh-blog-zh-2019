# 编译时计数

> 原文：<https://medium.com/swlh/counting-at-compile-time-6338aa471492>

![](img/19bbef7adcad770cbf70daab09ceaca6.png)

1, 2, 3, 4, …

编译时计数是世界上最简单的乐趣之一。我们使用了 scala 编译器，在里面加入了一些东西，让它做一些看起来不应该做的事情。

“编译时计数”的标准方法利用了一种通常称为`Nat`的类型。在图书馆里可以买到，比如[无形的](https://github.com/milessabin/shapeless)。我们有可用的类型，看起来像这样:`type _13 = <something>` —这代表…