# 使用 SQLite 测试 Laravel 5.8

> 原文：<https://medium.com/swlh/laravel-5-8-testing-with-sqlite-899d0fdd3494>

在测试迭代期间，使用内存中 SQLite 的“RefreshDatabase”特性，我们能够在不影响实际应用数据库的情况下实现数据库交互的快速测试迭代。

![](img/3782344908fc774c7e7bc748d58099f6.png)

# 先决条件

本文假设您已经有了一个在 Laravel 5.8 上运行的项目，并且您现在想要执行快速的单元/特性测试迭代，而不是…