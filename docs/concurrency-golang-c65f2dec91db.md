# 并发== Golang

> 原文：<https://medium.com/swlh/concurrency-golang-c65f2dec91db>

## 在本文中，我试图通过以 MapReduce 方式解决字数问题来分析(解释)不同的并发设计模式。

## Google LLC 的开发人员构建了 Golang，试图在分布式设置中以可扩展的方式解决并发问题。

字数统计问题基本上要求对多个文档中的每个单词进行计数。这可能是直接…