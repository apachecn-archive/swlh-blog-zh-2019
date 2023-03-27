# 学习 JavaScript 基础知识-全球范围

> 原文：<https://medium.com/swlh/javascript-fundamentals-global-scope-71ba5e48dbae>

*JavaScript 基础知识*系列作用域、提升、闭包

![](img/687c88afbb5a1b0835d27f0ec27dfa69.png)

Photo by [Joshua Sortino](https://unsplash.com/@sortino?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如果你正把 JavaScript 视为一个游戏，并像我一样计划成为一名职业玩家，你需要完美地理解规则。为了帮助你，我计划写一系列关于 JavaScript 的可能是最重要的概念；*范围、吊装和关闭*，本文将讨论**全球范围**。

对于这个主题的一个简单入口，让我们定义什么是作用域，为什么作用域如此重要？范围是定义变量可访问位置的方法。如果你理解作用域，你可以让你的变量和函数在你想要的时候或者你想要的地方是可访问的，这给了你一个在 JavaScript 中应该有的控制。定义可能不太清楚，希望这些文章能让我们看得更清楚。在 JavaScript 中，有两种类型的范围；全局范围和局部范围。

我认为 MDN Web Docs 对全球范围给出了很好的解释；

> 在编程环境中，*全局作用域*是包含所有其他作用域并且在其中可见的[作用域](https://developer.mozilla.org/en-US/docs/Glossary/scope)。在客户端 JavaScript 中，全局范围通常是执行所有代码的网页。

这意味着，如果你在全局范围内定义了一个变量，你就可以在全局范围内使用这个变量，你可以在所有的范围内访问它。在全局对象内部创建的所有变量或函数都是全局对象的成员。这个全局对象根据 JavaScript 运行的环境而不同。在 Node.js 中，全局对象等于**全局**，而在浏览器中它是**窗口。**

要在浏览器中检查窗口对象，只需编写 window 并进入控制台。你可以看到它已经有很多了。那些是可用的内置的。他们中的一些人可能很熟悉。窗口对象不仅存储全局定义的变量，它还有许多内置的属性、方法等等。甚至我们经常使用的**文档**对象也是窗口对象的属性。

为了解释窗口对象和文档对象之间的区别，因为它可能会引起混淆，文档对象是这个比较中较小的一个。它表示作为 HTML 根的 DOM。Window 对象则完全不同，它保存了 window.history、window.location 和 window.document 这样的信息，同样，这种区别也可以从 BOM(浏览器对象模型)和 DOM(文档对象模型)的区别中捕捉到。

*BOM 是* JavaScript *与浏览器对话的方式。它涉及诸如屏幕、历史、导航器、位置和文档等对象。DOM 是所有 HTML 文档的表示，是 BOM 的一部分。*

窗口对象只是顶层对象。您不能创建比窗口对象层次更高的范围。它是显示页面的浏览器窗口的表示。它是由浏览器创建的，是浏览器对象而不是 JavaScript 对象。它包含所有内置的以及我们在全球范围内创建的。如果你想测试它，打开你的控制台，写一个全局函数；

```
function forTesting(){
  console.log(‘I am a global function’);
}
```

现在，编写窗口并进入控制台。向下滚动到 F，你可以看到窗口对象现在有了你的全局函数。另一种测试方法是，你可以这样调用你的函数；

```
forTesting()
// I am a global function
window.forTesting()
// I am a global function
```

如你所见，结果是一样的。

要了解窗口对象如何工作，您可以尝试打开和关闭一个窗口对象。打开控制台并编写；

```
window.open()
```

然后在打开的窗口内，在控制台中；

```
window.close()
```

也有一些方法可以让你用 Window 对象做一些很酷的事情；

```
screen.width // returns the width of the screen
screen.height // returns the height of the screen
location.href // returns the URL
location.hostname // returns the domain name
history.back() // goes back in the browser
history.forward() // goes forward in the browser
alert() // display alert box
prompt() // display dialog box
```

如果你想阅读更多关于 JavaScript 基础的内容，请阅读学习 JavaScript 基础系列。

1.  [全球范围](/swlh/javascript-fundamentals-global-scope-71ba5e48dbae?source=post_page---------------------------)
2.  [局部范围](/@smeyradvrn/javascript-fundamentals-local-scope-5841690ea6aa?source=post_page---------------------------)
3.  [范围、上下文、执行上下文](/@smeyradvrn/learn-javascript-fundamentals-scope-context-execution-context-9fe8673b3164?source=post_page---------------------------)

编码快乐！！！