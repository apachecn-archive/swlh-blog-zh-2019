# Laravel: reCAPTCHA v3 的简单方法

> 原文：<https://medium.com/swlh/laravel-recaptcha-v3-the-easy-way-1e395083201b>

## reCAPTCHA v2 太过时了…

![](img/eefb2bdb80a51bf02e2ce5a77761f758.png)

Photo by [Rock'n Roll Monkey](https://unsplash.com/@rocknrollmonkey?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在关于机器人的新闻中，我想你可能知道 Laravel 已经有了*一种阻止机器人的方法*对失败的登录使用[节流器](https://laravel.com/docs/5.8/authentication#login-throttling)。对于任何简单的应用程序来说，这通常就足够了，因为大多数繁重的应用程序逻辑都是在一个经过身份验证的用户之下。