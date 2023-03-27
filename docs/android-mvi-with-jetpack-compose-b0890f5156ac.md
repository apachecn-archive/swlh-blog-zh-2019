# 带有 Jetpack Compose 的 Android MVI

> 原文：<https://medium.com/swlh/android-mvi-with-jetpack-compose-b0890f5156ac>

![](img/2f4d40955abfea7ecaf9c8abf2a2dff5.png)

Photo by [Luca Nicoletti](https://unsplash.com/photos/qB-jqzSE0yk?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/@lnicolet?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

今年，在 Google I/O 2019 上，已经宣布了 JetPack Compose。它仍然是

> 在早期探索，前阿尔法阶段。

如[官方文档页面](https://developer.android.com/jetpack/compose)所述。在深入研究了演示预览(可以下载)之后，我完全同意这种说法。它缺少一些基本的组件，不适合生产，也不适合简单地迁移 perk 项目的某个部分。

但这并不重要。我对我们目前所得到的非常满意。我有机会尝试了一下，并检查了这种*构建应用程序 UI 的新方式的潜力。*

![](img/d18ff83c45b709bee1d8defd91468406.png)

Photo by [Andrik Langfield](https://unsplash.com/photos/ujx_KIIujRg?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/build?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在我看来，从 Flutter 开始，然后是这个库，Google 试图将 web 开发方式移植到移动应用程序中。在 Flutter 中，一切都是一个`Widget`，这里组件被声明为函数。

这可能是一个很好的开始，当然也有助于学习 Flutter。一旦掌握了通过“组合”组件来构建 ui 的思想，切换到 Flutter (React)模式就相当简单了。构建组件背后的范例在这两者之间并不遥远。

# 简要介绍

JetPack Compose 允许您声明

> UI 组件，包括绘制和创建自定义布局。

这意味着您可能有一个不返回任何内容的函数——从字面上看，该函数没有返回类型:

但是这个函数，由于有了`@Composable`注释，允许我们声明一个“视图”,这个视图可以通过活动的`setContent()`来设置(实际上是画出来的)。

这是一个初始化可组合小部件树并将其包装在一个`FrameLayout`中的方法。

# 通常的方式

我计划向您展示的是，我们如何以一种奇特的方式使用这个新的 Jetpack Compose 来实现 **MVI** 模式。
现在，我们通常这样做来处理安卓系统中的 **MVI** :

我们通过`LiveData<MVIViewState>`发布从远程或本地存储中获取的任何内容(缓存数据！)这样:

然后我们从我们的活动中观察到`LiveData`:

`View`负责完全呈现*UI*，这有时会导致在`View`中也使用逻辑， *if-else* 语句来显示或隐藏某些东西，而那个`when`块，真的令人沮丧，不是吗？为什么`View`要检查状态，并以不同的方式呈现一个状态？`View`应该接收一个`State`并呈现它，不执行任何其他操作(以纯函数的方式)。

# 让我们改变我们建造东西的方式！

如果我们可以改变这种情况，给`ViewModel`，或者更好的说法是`ViewState`——构建 *UI* 的能力，而让`View`只渲染它，会怎么样？多亏了 Jetpack Compose，这才成为可能。

HURRAY!

那么，我们需要改变什么呢？
这个`MVIViewState`类会有一点不同:

这个类相当冗长，这是有原因的:`ViewState`应该表示`View`，所以它应该**知道**如何渲染它，对吗？
加入一项

函数给`ViewState`，我们强制每个子类定义它，这意味着每个子类将知道如何“呈现”它的状态。这样，**在`ViewState`类的**里面，我们相应地构建了`ViewState`类的`View`。这将允许我们有一个更精简的**`View`(活动或片段)。**

**那么，`ViewModel`级或者`Activity/Fragment`级呢？嗯，`ViewModel`基本保持不变:**

**Okok，还有那个`View`？**

**这就是了！是不是*漂亮*？**

**我个人 ***爱*** 怎么看也只是变得真 ***精*** 而基本上什么都不做。i̶t̶**̶o̶b̶s̶e̶r̶v̶e̶s̶**̶t̶h̶e̶̶l̶i̶v̶e̶d̶a̶t̶a̶̶a̶n̶d̶̶t̶h̶a̶t̶'̶s̶̶i̶t̶.̶̶t̶h̶r̶o̶u̶g̶h̶̶t̶h̶e̶̶s̶e̶t̶c̶o̶n̶t̶e̶n̶t̶̶{̶}̶̶f̶u̶n̶c̶t̶i̶o̶n̶̶f̶r̶o̶m̶̶t̶h̶e̶̶j̶e̶t̶p̶a̶c̶k̶c̶o̶m̶p̶o̶s̶e̶̶l̶i̶b̶r̶a̶r̶y̶,̶̶w̶e̶'̶r̶e̶̶n̶o̶w̶̶a̶b̶l̶e̶̶t̶o̶̶d̶o̶̶t̶h̶i̶s̶̶m̶a̶g̶i̶c̶̶t̶r̶i̶c̶k̶！̶**

**多亏了利兰·理查森和他的建议，现在已经变得更好了。**

**它不是多次调用`setContent{}`，而是创建视图的基本组件:`CraneWrapper`、`Scaffold`和`AppBar`，然后*通过观察`ViewState`上的变化，在`Scaffold`内渲染*到`View`的部分，也就是*渲染*的部分。**

## **下降趋势**

**但是也有一些缺点:**

*   **视图不再负责构建布局**
*   **没有 XML 文件，所以您必须找到`ViewState`类，搜索您想要检查的正确状态，并一个接一个地读取它构建的每个组件**
*   **布局检查器不起作用。 ***等等，什么？！？*** 是的，没有。这是因为 Jetpack Compose 并不真正在视图层次结构中创建视图，它只是在画布上绘制了视图，所以基本上你是在检查视图。**

## **我们在乎吗？**

**一点也不。让我解释一下。上面列出的那些缺点是真正的缺点。我的意思是，如果没有布局检查器，我会一直试图找出为什么从 12 月份开始点击一个组件就不起作用了。
但是由于`View`不再负责构建布局，你可以很容易地检查它是如何构建的。您将不再有任何隐藏的加载器、隐藏的回收器、隐藏的自定义视图。所有你的用户需要看到的，将被构建到“*视图层次*”并绘制在画布上。你将最终拥有一个**所见即所得**。**

# **结论**

**就是这样！这只是一个简单的介绍，我预计它将是结合 Android 世界中的 **MVI** 模式的 Jetpack Compose 的开发！**

**希望你喜欢阅读！感谢每一个反馈(这是我的第一篇博文:D)。**