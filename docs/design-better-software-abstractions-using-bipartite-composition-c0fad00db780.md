# 使用二分组合设计更好的软件抽象

> 原文：<https://medium.com/swlh/design-better-software-abstractions-using-bipartite-composition-c0fad00db780>

# 问题

你写了一个对象或类。这有点复杂，你想要迭代的方法来更好的抽象你的代码，但是不确定如何开始。有关于好的抽象看起来像什么的文档，但是没有具体的迭代步骤。

> 深入自己的代码会让你对显而易见的选择视而不见。

# 解决办法

创建实例方法的[二分图](https://en.wikipedia.org/wiki/Bipartite_graph)和[成员变量](https://en.wikipedia.org/wiki/Member_variable)。在方法和…