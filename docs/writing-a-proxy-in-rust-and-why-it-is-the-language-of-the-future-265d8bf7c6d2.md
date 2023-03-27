# 用 Rust 写代理，为什么它是未来的语言

> 原文：<https://medium.com/swlh/writing-a-proxy-in-rust-and-why-it-is-the-language-of-the-future-265d8bf7c6d2>

我用 Rust 写了一个小代理已经有一年了，这是我用这种语言做的第一个项目之一，我在编写它的过程中学到了很多。总而言之，这个代理的主要目标是简单易用，并且易于用中间件扩展。它针对 HTTP APIs，可以用在很多服务的前面。这个代理已经在多个项目中使用，有微服务架构等等…

# 为什么是这个代理？