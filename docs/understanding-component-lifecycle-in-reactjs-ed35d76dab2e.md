# 了解 ReactJS 中的组件生命周期

> 原文：<https://medium.com/swlh/understanding-component-lifecycle-in-reactjs-ed35d76dab2e>

![](img/faced30ea3ad85c3cc1156f828c2661f.png)

您在 React 应用程序中看到的一切都是组件或组件的一部分。在 React 中，组件被设计成遵循生命的自然循环。他们出生(创造)，成长(更新)，最后死亡(删除)。这被称为**组件生命周期**。

对于组件生命周期的每个阶段，React 提供对某些内置事件/方法的访问，这些事件/方法被称为**生命周期挂钩**或**生命周期方法**。这些方法使您有机会控制和操作组件如何对应用程序中的变化做出反应。

让我们来看看组件生命周期中的每个阶段:

## 预安装(初始化)

组件是一个 JS 类。像任何类一样，它有一个`constructor`函数，调用它来设置东西。它通常设置状态和道具。

## 增加

初始化完成后，组件的一个实例被创建并挂载到 DOM 上。使用其初始状态，组件第一次呈现在页面上。在这个阶段，我们有两种可用的生命周期方法:`componentWillMount`和`componentDidMount`。

在`constructor`被调用之后，`componentWillMount`在 `render`之前被调用，并且在一个生命周期中被调用一次。这种方法用得不多——甚至[的 React 文档](https://reactjs.org/docs/react-component.html#componentwillmount)提到你在这里可以做的任何事情都最好用`constructor`或`componentDidMount`方法来完成。

如果您试图在这个方法中使用`this.setState`进行任何 API 调用或数据更改，DOM 中不会发生任何事情(没有更新),因为在 render 方法之前调用了`componentWillMount`。

`componentDidMount`是在之后被称为*的`render`方法。和`componentWillMount`一样，一个生命周期调用一次。因为 render 方法已经被调用，所以我们可以访问 DOM。您可以使用该方法来设置任何长期运行的流程或异步流程，例如获取和更新数据。*

## 更新

每当组件的状态和属性从 React 组件内部或通过 api 或后端发生变化时，组件就会通过在页面上重新呈现来更新。状态和属性的变化取决于用户与组件的交互或者是否有新数据传入。

此阶段可用的生命周期方法有:

1.`componentWillReceiveProps`:当父元素传递给组件的属性发生变化时，这个方法被调用。

2.`shouldComponentUpdate`:这个方法在组件将要重新呈现之前被调用。它决定组件是否应该更新。默认情况下，它返回 true。你可以通过使用`nextProps`和`nextState`参数来比较新旧道具和状态，如果道具和/或状态的变化不影响显示给用户的内容，可以避免不必要的重新渲染。

3.`componentWillUpdate`:这个方法在`shouldComponentUpdate`完成之后，新组件渲染之前被调用。使用此方法的一些示例包括:如果您在重新渲染之前以及在道具和/或状态更新之后需要执行任何计算，或者如果您需要更新与第三方库的集成。像`shouldComponentUpdate`一样，它也接收像`nextProps`和`nextState`这样的参数。

4.`componentDidUpdate`:这个方法在组件重新渲染后被调用。您将可以使用`prevProp`和`prevState`访问以前的属性和状态，以及当前的属性和状态，并且您可以使用此方法更新任何第三方库，如果它们由于重新渲染而需要更新的话。

## 卸载

这是组件生命周期的最后一个阶段。在卸载阶段，组件将从页面中删除。这个阶段唯一的生命周期方法是`componentWillUnmount`，它在组件被删除之前被调用。用于清除在`componentDidMount`中设置的任何内容。例如，删除`componentDidMount`中定义的任何定时器。

## 贬低生命周期挂钩

React 团队已经决定在 React 17 中弃用一些生命周期方法。ReactJS 团队最近的一篇博客文章揭示了组件生命周期方法的未来。

三种生命周期方法`componentWillMount`、`componentWillRecieveProps`、`componentWillUpdate`即将被弃用。然而，它们并没有完全消失，因为你可以将它们与`UNSAFE_componentWillMount`、`UNSAFE_componentWillRecieveProps`、`UNSAFE_componentWillUpdate`一起使用。

**它们为什么不安全？**

最初的生命周期模型并不打算用于即将到来的一些特性，比如异步呈现。随着异步渲染的引入，这些生命周期方法中的一些在使用时会变得不安全。

例如，异步渲染会导致`componentWillMount`触发组件树的多重渲染。这使得它不安全。

## 摘要

了解组件的生命周期将使您能够在创建、更新或销毁组件时执行某些操作。不是每个方法都需要在你构建的每个组件中使用。使用它们的好处是有机会首先决定组件是否应该更新，并相应地对 props 或状态变化做出反应。

感谢阅读！

## 参考

[](https://reactjs.org/docs/react-component.html) [## 做出反应。成分-反应

### 该页面包含 React 组件类定义的详细 API 参考。它假设您熟悉…

reactjs.org](https://reactjs.org/docs/react-component.html) [](https://www.freecodecamp.org/news/how-to-understand-a-components-lifecycle-methods-in-reactjs-e1a609840630/) [## 如何理解 ReactJS 中组件的生命周期方法

### 在本文中，我们将探讨 ReactJS 的生命周期方法。但是，在做出反应之前…

www.freecodecamp.org](https://www.freecodecamp.org/news/how-to-understand-a-components-lifecycle-methods-in-reactjs-e1a609840630/)  [## 了解 React 组件生命周期

### 了解组件的生命周期将使您能够在创建组件时执行某些操作，或者…

忙人. github.io](https://busypeoples.github.io/post/react-component-lifecycle/) [](/front-end-weekly/how-to-use-react-lifecycle-methods-103fc3b1711) [## 如何使用 React 生命周期方法

### React 附带了几个生命周期方法，如果您不知道哪一个适合您的场景，它会变得很混乱…

medium.com](/front-end-weekly/how-to-use-react-lifecycle-methods-103fc3b1711)