# React 测试:让 jest 很好地使用 webpack 静态资产导入

> 原文：<https://medium.com/swlh/react-testing-getting-jest-to-play-nicely-with-webpack-static-assets-imports-74b1c1472234>

![](img/700d77eeefa23ef6f778b6c1b4364804.png)

Photo by [Chris Liverani](https://unsplash.com/@chrisliverani?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在长期使用公共文件夹中单独的 *css* 文件并直接从 index.html 的*引用它们之后，我们最近切换到将我们的 *css* 与组件放在一起，并使用 *webpack* 将其导入到组件中(使用 *css-loader* 和 *style-loader* )。我们不希望它打破我们现有的设置，因为为什么，还有谁在使用 *css* 文件…*