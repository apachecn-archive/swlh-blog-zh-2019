# JavaScript 对象和私有数据

> 原文：<https://medium.com/swlh/javascript-objects-and-private-data-94548bf00e86>

![](img/10f5ef11fd016f3da7f668dfd74a1fc0.png)

Photo by [Waldemar Brandt](https://unsplash.com/@waldemarbrandt67w?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在这篇简短的文章中，我将尝试使用 JavaScript 的两种对象创建模式来说明 JavaScript 在处理私有数据方面的差异。这样做，我希望在需要封装私有数据时，在这两种模式之间进行一些权衡。

首先，我们将在介绍每个对象如何处理私有变量之前，简要地检查一下正在讨论的两种对象创建模式。对于一个…