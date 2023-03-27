# 理解 Rust 中的闭包。

> 原文：<https://medium.com/swlh/understanding-closures-in-rust-21f286ed1759>

# 摘要

*   闭包是函数指针(`fn`)和上下文的组合。
*   没有上下文的闭包只是一个函数指针。
*   具有不可变上下文的闭包属于`Fn`。
*   具有可变上下文的闭包属于`FnMut`。
*   拥有其上下文的闭包属于`FnOnce`。

# 理解 Rust 中不同类型的闭包。