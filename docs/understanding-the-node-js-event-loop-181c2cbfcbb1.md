# 了解 Node.js 事件循环

> 原文：<https://medium.com/swlh/understanding-the-node-js-event-loop-181c2cbfcbb1>

![](img/e3a5386afe31bf6301be0dad310649a0.png)

本文帮助您理解 Node.js 事件循环是如何工作的，以及如何利用它来构建快速应用程序。我们还将讨论您可能遇到的最常见的问题，以及它们的解决方案。

# 问题是

网站背后的大部分后端不需要做复杂的计算。我们的程序大部分时间都在等待磁盘…