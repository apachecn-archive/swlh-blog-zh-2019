# 用 djb2 算法实现 C++中的哈希表

> 原文：<https://medium.com/swlh/hash-tables-in-c-with-the-djb2-algorithm-21f14ba7ca88>

![](img/d7738b1ac2cce626a3d39f72b1d34914.png)

Photo by [Roman Bozhko](https://unsplash.com/@romanbozhko?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 什么是哈希表？

在计算机科学中，哈希表是一种实现链表数组来存储数据的数据结构。使用哈希算法，哈希表能够计算索引来存储字符串不同的键值对。

今天，我将演示如何用 C++创建这种数据结构。这是我用…写的第一份申请