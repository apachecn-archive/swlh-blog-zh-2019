# 你怎么能相信 JWT 是真迹呢？

> 原文：<https://medium.com/swlh/how-can-you-trust-a-jwt-to-be-authentic-2bd5124b243a>

![](img/e9a6f425864e829abcdd7b530fa692c1.png)

在这篇文章中，我将分享一些我对 JWT·托肯的了解。具体来说，我将介绍 JWT 的结构，以及如何通过验证签名来信任令牌中的信息是真实的。

# JWT 令牌的结构。

一个 Json Web 令牌(JWT)包含某些声明或事实，如发布者、用户、所有者等