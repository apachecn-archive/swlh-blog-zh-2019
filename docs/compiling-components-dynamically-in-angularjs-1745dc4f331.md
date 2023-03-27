# 在 AngularJS 中动态编译组件

> 原文：<https://medium.com/swlh/compiling-components-dynamically-in-angularjs-1745dc4f331>

![](img/ce8ab49592aa3c8d659f2d5aad1b7624.png)

Photo by [Suzanne D. Williams](https://unsplash.com/@scw1217?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

当您开发一个大规模的基于组件的应用程序时，它经常与服务器通信并从服务器接收数据，页面、特性和组件的结构可能自然地依赖于您接收的数据。

如果您将组件模块化为独立的小单元，每个单元负责页面中的一个区域或一部分，那么您可能会有一个充满依赖项的 package.json。大部分…