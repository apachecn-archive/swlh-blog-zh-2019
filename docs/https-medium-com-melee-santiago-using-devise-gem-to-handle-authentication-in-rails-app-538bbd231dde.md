# 使用 Devise Gem 处理 Rails 应用程序中的身份验证

> 原文：<https://medium.com/swlh/https-medium-com-melee-santiago-using-devise-gem-to-handle-authentication-in-rails-app-538bbd231dde>

![](img/5167a436ae61a74ad79c3678d2e3f838.png)

Photo by [Jon Tyson](https://unsplash.com/photos/XmMsdtiGSfo?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/sign?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

O 任何应用程序最重要的功能之一就是身份验证。最简单的形式是，您希望您的用户能够安全地注册、登录和注销。你可以依靠 Rails 的内置方法`has_secure_password` 来提供这种功能，但是你需要自己添加必要的密码验证和辅助方法。如果您的用户忘记了密码怎么办…