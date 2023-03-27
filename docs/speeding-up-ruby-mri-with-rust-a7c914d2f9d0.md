# 用 Rust 加速 Ruby MRI

> 原文：<https://medium.com/swlh/speeding-up-ruby-mri-with-rust-a7c914d2f9d0>

![](img/8357fe5d793dd35105ef5c220597134f.png)

让我先说我真的很喜欢 Ruby。我倾向于同意这样的说法，Ruby 是为了开发者的快乐而优化的。然而，没有什么是免费的。编程狂喜是一把双刃剑，编写缓慢的 Ruby 既容易又令人愉快。

你可以非常非常努力地从你的程序中榨取尽可能多的 CPU 资源，方法是将内存分配减少到最低限度…