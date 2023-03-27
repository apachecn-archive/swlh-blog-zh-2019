# 异步 Clojure

> 原文：<https://medium.com/swlh/asynchronous-clojure-a59fa0f9bda0>

## 通道和 core.async 库入门

Core.async 是 Clojure 的主要库，用于异步编程和通信。它为您做了许多繁重的工作，从而使异步编程变得容易得多。这个图书馆有几个目标，你可以在这里阅读，但主要目的是:

*   为独立的活动线程提供设施，通过队列式*通道*进行通信
*   支持真正的线程和线程池的共享使用(以任何组合)，以及…