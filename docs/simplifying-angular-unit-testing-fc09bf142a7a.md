# 角形组件的简单单元测试

> 原文：<https://medium.com/swlh/simplifying-angular-unit-testing-fc09bf142a7a>

![](img/d5d6e54271e41149697c64b8636cf095.png)

If you wish to make an apple pie from scratch, you must first invent the universe. — Carl Sagan

我总是发现角度组件单元测试的设置相当复杂。当组件在层次结构中处于较高的位置，并且包含一个或多个子组件时，这是一个特别棘手的问题，每个子组件可能都有自己的子组件。更糟糕的是，当这些组件中的任何一个使用 Angular DI 机制注入任何依赖项时，您会对所有的模块和提供者感到疑惑…