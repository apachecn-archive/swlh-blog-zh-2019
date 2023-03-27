# 大型 monorepos 的高效 Docker 构建

> 原文：<https://medium.com/swlh/incremental-docker-builds-for-monolithic-codebases-2dae3ea950e>

[阅读我博客上的原文](https://cjolowicz.github.io/posts/incremental-docker-builds-for-monolithic-codebases/)

![](img/b83f0cd273f48eb28cd61f2236e9f45d.png)

Photo by [Thomas Kelley](https://unsplash.com/photos/t20pc32VbrU?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/whale?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

作为一名开发整体代码库的开发人员，你如何使用 [Docker](https://www.docker.com/) 来构建和部署其中包含的项目？如果采用天真的方法，很快就会遇到图像膨胀和频繁重建整个源代码树的问题。