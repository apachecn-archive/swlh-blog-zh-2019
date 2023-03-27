# 接下来打字。使用 TypeScript 的 JS 组件

> 原文：<https://medium.com/swlh/typing-next-js-components-using-typescript-2f1d0dc30c4c>

![](img/722d3b16d3fbafae0586e81a17147755.png)

> **注:**本帖涵盖打字 Next。使用 v9 之前版本的 JS 应用程序。从版本 9 开始，接下来。默认情况下，JS 有自己的类型，它们的名称可能与 DefinitelyTyped package 中使用的不同。如果你用 Next。JS 版本 9 及以上，请参考[官方文档](https://nextjs.org/docs#typescript)。对于早期版本，请继续阅读:)

在这篇文章中，我们接下来将讨论打字。JS 组件。接下来我们将使用[这个。JS](https://github.com/koss-lebedev/nextjs-typescript) 应用程序，连接到 Reddit API 并显示给定子编辑中热门文章的列表。现在，主要组件`Posts.tsx`没有任何类型安全，所以我们将修复它。

*注意:*项目已经包含了配置的 TypeScript，所以如果你不知道怎么做，请在这里阅读官方指南[，就像安装一些依赖项和放入一些配置文件一样简单。](https://github.com/zeit/next-plugins/tree/master/packages/next-typescript)

现在让我们从 GitHub 克隆[项目](https://github.com/koss-lebedev/nextjs-typescript)并开始吧。

## 功能组件

首先，让我们看看一般情况下我们是如何输入 React 组件的。

如果我们把组件写成函数，我们可以使用 React 库中的`FunctionComponent`类型。此类型采用描述道具形状的泛型类型参数。我们的`Posts`组件接受一个子编辑名和一个帖子列表，所以`props`对象看起来像这样:

现在当我们把道具分解成`posts`和`subreddit`的时候，我们得到了全类型安全。很漂亮，对吧？

现在我们来看下一个。JS 组件。

## NextFunctionComponent

接下来的一件事。JS 组件不同的是一个静态的`getInitialProps`函数。如果我们试图将它赋给我们的常规 React 组件，我们将得到一个类型错误:

为了解决这个问题，我们需要使用一个特殊的组件类型。JS 包名为`NextFunctionComponent`。这种类型用 Next 扩展了标准 React 的`FunctionComponent`类型。特定于 JS 的静态生命周期方法(实际上，只有一种方法)。所以现在我们的代码看起来像这样:

为了使我们的类型更加健壮，我们可以通过*推断从`getInitialProps`函数返回的`props`的形状，而不是手动定义它们。为此，首先，我们要将`getInitialProps`函数提取到一个单独的变量中。当我们开始推断道具的形状时，需要这一步来避免循环类型引用:*

接下来，我们可以使用`ReturnType`帮助器类型来获取从`getInitialProps`函数返回的值的类型:

由于`getInitialProps`函数是异步的，返回类型将是一个承诺，所以我们也需要提取它的值。我们可以定义一个全局助手类型，它将使用条件类型魔法来解开我们的承诺:

现在我们可以把所有东西放在一起:

## NextContext

让我们通过从查询字符串中提取一个子编辑的名称来使这个例子更有趣。

为了实现这一点，我们可以使用一个`context`参数，该参数被传递给到目前为止我们还没有使用过的`getInitialProps`函数。我们将使用`NextContext<T>` type 来键入这个参数。类型`T`允许我们指定查询字符串中的参数(在我们的例子中是`subreddit`):

我们在`getInitialProps`函数中得到了类型安全，但是现在我们遇到了另一个类型错误，即`Context`的不兼容类型。

原因是默认情况下`getInitialProps`期望一个上下文是通用类型`NextContext`，但是我们指定了一个更严格、更具体的类型— `NextContext<{ subreddit: string }.`为了解决这个问题，我们需要向`NextFunctionComponent`类型传递更多的类型参数。它的完整签名如下所示:

如您所见，`NextFunctionComponent`最多可以有 3 种类型— `Props`、`InitialProps`和`Context`。`InitialProps`应该只包含从`getInitialProps`函数返回的道具。`Props`应该包含组件可以访问的所有道具，包括自己的组件道具、通过更高阶组件传递的道具(比如来自 Redux 的`connect`)，加上初始道具。最后，`Context`指定了组件所使用的上下文的形状。

当我们把所有的东西放在一起，我们会得到一个完整的下一步。JS 组件:

您可以在[“final”分支](https://github.com/koss-lebedev/nextjs-typescript/tree/final)中找到完全类型化组件的源代码。

## 结论

在本文中，我们探索了如何键入 Next。JS 组件。该方法不同于键入常规的 React 功能组件，因为接下来是特殊的`getInitialProps`函数。用 JS 编写的 props 数据服务器端。因此，我们需要使用 Next 附带的特殊的`NextFunctionComponent`和`NextContext`类型。JS 打字包。

PS:如果你想知道为什么我们到处使用类型别名而不是接口，一定要看看这篇文章。

**不要脸的塞**:如果你想在不写一大堆样板文件的情况下学习如何使用 Redux，可以在 Youtube 上查看我的[“带重配的状态管理”](http://bit.ly/rematch-course)课程。