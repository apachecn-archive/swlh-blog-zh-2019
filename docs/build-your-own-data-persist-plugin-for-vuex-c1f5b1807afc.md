# 为 Vuex 构建自己的数据持久插件

> 原文：<https://medium.com/swlh/build-your-own-data-persist-plugin-for-vuex-c1f5b1807afc>

## 给 VUE 的简单建议。JS 开发人员

如果你像我一样开始在你的项目中使用 Vue.js，你可能会陷入这样一个境地:你需要*不相关的*(没有父子关系)组件来*彼此交谈*(共享道具)。

![](img/96ac3c0c582583099de5186098748774.png)

Photo by [v2osk](https://unsplash.com/@v2osk?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/aurora?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

## 第一种方法:全局事件总线