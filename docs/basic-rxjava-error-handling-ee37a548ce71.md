# 基本 RxJava 错误处理。

> 原文：<https://medium.com/swlh/basic-rxjava-error-handling-ee37a548ce71>

![](img/7e08e773f853c2164b4c32362087b6f5.png)

在 RxJava 中管理错误时，我们唯一需要知道的是什么吗？对于 RxJava 的非常基本的用法来说，这可能就是您所需要的全部，但是当您的业务逻辑是用 Rx 创建的时，它就没有多大帮助了。

那么，当`flapMap`或`combineLatest`中的可观察对象抛出异常时，我们如何处理错误并控制数据流呢？

# onErrorReturnItem/onErrorResumeNext