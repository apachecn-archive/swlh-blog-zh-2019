# 不要让 Api 数据构造你的 JavaScript 应用程序

> 原文：<https://medium.com/swlh/dont-let-api-data-structure-your-javascript-application-7fa7fd5a590f>

## 规范化 API 数据的属性名

![](img/04ce8ab93d3deba78156140b92f143ae.png)

Photo by [Daniel Cheung](https://unsplash.com/photos/dDppsuM_UpE?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

当我创建应用程序时，一致性对我来说很重要。所以我喜欢遵循的一个标准是把我所有的代码都放在`camelCase`里。在同一个应用程序中使用新的和遗留的 API 可能会令人沮丧。每个 API 可以返回不同的案例类型。比如:(`kebab-case`、`snake_case`……