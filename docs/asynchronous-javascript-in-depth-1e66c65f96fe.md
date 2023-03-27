# 深入了解异步 Javascript。

> 原文：<https://medium.com/swlh/asynchronous-javascript-in-depth-1e66c65f96fe>

来自 Ruby 这样的同步代码执行背景，我总是对 javascript 如何通过保持单线程以异步方式执行代码感到困惑。这种异步代码执行是非常重要的面向性能的计算方式，现在被许多语言所采用。虽然有些语言仍然不支持它，但有像[react vex](http://reactivex.io/)这样的第三方库可以帮助你实现异步工作流。

在深入 javascript 异步流之前，我们将了解同步流和异步流的区别