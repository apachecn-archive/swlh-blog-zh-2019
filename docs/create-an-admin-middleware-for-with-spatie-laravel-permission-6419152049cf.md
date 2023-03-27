# 使用 spatie/laravel-permission 为 Laravel 创建一个管理中间件

> 原文：<https://medium.com/swlh/create-an-admin-middleware-for-with-spatie-laravel-permission-6419152049cf>

> 注意:本教程适用于 Laravel 7 或更早的版本

![](img/fe187ed2ce6f528a848534534ceb11e1.png)

Photo by [CMDR Shane](https://unsplash.com/@cmdrshane?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

虽然有很多关于这个主题的文章，但我决定为我未来的自己写一篇文章，并与大家分享我通常用来根据特定角色分离应用程序的方法。

> 中间件为…提供了一个方便的机制