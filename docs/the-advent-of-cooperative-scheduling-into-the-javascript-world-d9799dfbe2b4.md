# JavaScript 世界中合作调度的出现。

> 原文：<https://medium.com/swlh/the-advent-of-cooperative-scheduling-into-the-javascript-world-d9799dfbe2b4>

比赛开始了！

W3C 已经发布了一个提议的标准，使 web 应用程序能够协同调度任务。它基于`requestIdleCallback`功能，该功能允许您将任务推迟到后台。这个 API 可以用来制作流畅的动画，同时在幕后进行复杂的计算。

## 什么是排班？

调度允许并发任务共享 CPU 时间，而无需实际并行运行。它可以在两个…