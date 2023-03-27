# 我的 React 门户之旅

> 原文：<https://medium.com/swlh/my-journey-into-the-react-portal-b13b1c8d3339>

![](img/18665ee013e11b54e6343457c896939e.png)

我最近需要在 React 应用程序的其他地方呈现一条 flash 消息。据我所知，我有三个选择:

1.  使用 Redux 和一个连接的组件来呈现 flash 消息。
2.  使用上下文，由于强大的`useContext`钩子，它比以往任何时候都更容易使用。
3.  使用我不久前在 React 文档中看到的一个叫做门户的东西。