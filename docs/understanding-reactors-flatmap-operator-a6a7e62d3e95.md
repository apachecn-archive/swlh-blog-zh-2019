# 了解反应的。flatMap()运算符

> 原文：<https://medium.com/swlh/understanding-reactors-flatmap-operator-a6a7e62d3e95>

*。flatMap()* 是 Reactive 运算符中最神秘的一种。虽然很难理解，但一旦我们确切地理解了它是如何工作的，它就成为我们工具箱中一个极其强大的操作符。在这篇文章中，我们将试图剖析它，了解它做什么，为什么它做什么。注意:我使用 Project Reactor 进行演示，但 RxJava 的*。flatMap()* 也是同样的工作方式。在这篇文章中，你所需要知道的关于 Reactor 的一切就是一个*单声道*代表一个包含单个事件的流，而一个*通量*代表一个包含多个事件的流。