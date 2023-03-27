# 通过做 Android 服务和 IntentServices 来学习(1/3)

> 原文：<https://medium.com/swlh/learn-by-doing-android-services-and-intentservices-1-3-920d0618aaea>

这是一个由 3 部分组成的系列。

第 2 部分— [绑定服务](/@shivamdhuria/learn-by-doing-android-services-and-intentservices-2-3-6969678f81b5)

第 3 部分— [意向服务](/@shivamdhuria/learn-by-doing-android-services-and-intent-services-3-3-3674cb09a4d6)

> 什么是服务？

*   它是一个应用程序组件，运行 ***而不与用户直接交互*** 。它没有用户界面。
*   它主要用于短期任务。然而，对于运行时间较长的任务，我们可能需要在服务中创建另一个线程，因为默认情况下，服务运行在调用组件进程的**主线程**上，这可能会导致 ANR。