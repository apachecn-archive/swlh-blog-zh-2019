# Javascript:隐藏部分(隐式和自动)

> 原文：<https://medium.com/swlh/javascript-the-hidden-parts-implicit-and-automatic-6dcd505f153d>

![](img/7c505cedf433220f16c03cdfd6ab9e60.png)

Photo by [Isis França](https://unsplash.com/@isisfra?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

有相当多的 Javascript 代码你不需要输入。从分号到隐式参数。本文将展示一些例子，这些例子要么是被设计遗漏的，要么是为了帮助编码者而被纠正的。

## 自动分号插入

不仅仅是你！其他 Javascript 开发人员也很懒。尽管；不是每个人都想在每一行结束时都输入。所以 ECMA 国际在制作 ES5 的过程中决定消除这种压力。这些规则后来在 ES2015 (ES6)中得到扩展，如果需要，可以在这里阅读[。](https://www.ecma-international.org/ecma-262/6.0/index.html#sec-automatic-semicolon-insertion)

对于那些不想阅读他们所分解的规则的人来说:

1.  它将被插入到一个有效代码段的行结束符之前(就在您点击 return 换一个新行之前)
2.  如果需要，它将在}之前插入
3.  下一行的开头不是(或许多其他免除之一

规则一免除了一些事情，比如当你在回车之后有一个{ right，即使在它自己的行上。然而，如果没有像(或{。这样做的实际效果是，您最终可能会键入:

`return
a+b`

并添加分号作为

```
return;
a+b; 
```

还有更复杂的例外情况，例如，当有 else 时，if/else 语句不会在 if 的{}后面插入分号，因为这会违反 if/else 的工作方式。ECMAscript 中有有效语法这种东西，自动分号不会违反它。

[布拉德利·布莱斯维特](http://www.bradoncode.com/blog/2015/08/26/javascript-semi-colon-insertion/)就此写了一篇很棒的文章，其中包括整个事情的奇妙图表。我强烈推荐通读并关注他关于依赖自动逗号插入的警告。

## 隐性强制

您可能知道，JS 中没有显式类型。如果你愿意，你的变量可以是一个字符串，下一个是一个数字。我建议不要选择这样做，因为这会让大多数参与者感到困惑。

那么什么是隐性强制呢？大致是这样的:

```
"2" + 1 = "21"
"2" - 1 = 1
1 + 1 + "2" = "12"
true + true = 2
3 + [1] = "31"
3 * [ 1 ] = 3
3 * [ 1, 2] = NaN
```

您可能以前在一些关于 Javascript 怪癖的恶魔中，或者在您自己的工作中见过类似的东西。这是有原因的，那就是 Javascript 真的很想工作。它希望通过忽略当您试图对两个不同的类型(如 string 和 number)进行操作时出现的问题来帮助您。

在 Javascript 中,+操作符被称为[重载](https://en.wikipedia.org/wiki/Operator_overloading),这意味着它根据周围的情况做不同的事情。这种特殊的重载并不是 Javascript 独有的，我试图找出它的来源却一无所获。如果有人知道，评论或给我发消息，我会更新信用。因此，这种混乱的本质部分在于，如果你使用+的话，它在默认情况下不会被认为是一个数学运算符，而像-、*和/则是。

不过回到手头的话题。为了向您展示这里发生了什么，让我用 Javascript 实际上在做什么来重写上面的内容:

```
"2" + String(1) = "21"
Number("2") - 1 = 1
String(1 + 1) + "2" = "22"
String(3) +[ 1 ].join() = "31" 
3 * Number([ 1 ].join()) = 3
3 * Number([ 1, 2 ].join()) = NaN //this comes from Number("1,2") which equals NaN
```

这里还有另一个潜在的逻辑。Truthy/Falsy 是 Javascript 值被强制转换为布尔值时的描述。Javascript 中的一切都是真实的，除此之外:

*   错误的
*   0
*   空
*   “”
*   不明确的
*   圆盘烤饼
*   -0

你可能已经用过了。例如，如果你检查某个未定义的东西，你可以写:

```
if(testVal){
  throw error
}
```

这是通过转换值`boolean(testVal)`将值与`true`进行==比较，因为等式是一个布尔运算符。早些时候我举了一个例子`true + true = 2`，它就像写作`Number(true) + Number (true) = 2`。加号将它们转换成数字，以便进行加法运算。要了解更多这种疯狂，请看这张[图](https://dorey.github.io/JavaScript-Equality-Table/)，它展示了如何将所有值与 a ==进行比较。

## 隐性回报

在 ES2015(ES6)中引入了箭头功能的概念。除了看起来非常酷(主观)和减少你需要绑定`this`的次数，特别是在 React 中，他们还引入了隐性回报的概念。

它们是这样写的:

```
const testFunc = () => 'It worked!';
```

= >也是不会应用隐式分号的情况之一。一旦添加了该字符串，它就会。有几种方法可以打破这一点:

```
const testFunc2 = () => {'It worked!'};const testFunc3 = () => {return 'It worked!'};
```

这里添加的括号({})改变了函数的处理方式。一旦它们被添加，返回必须被添加，否则返回将是未定义的。不过，这里还有其他问题:

```
const testFunc4 = () => {workingVal:'It worked!'};const testFunc5 = () => ({workingVal:'It worked!'});
```

猜猜这些函数在哪个位置实际上会返回一个对象，哪个函数在被调用时会返回未定义的对象。答案是 testFunc5。括号被误认为是函数体的开口。圆括号防止了这种情况。

要使隐式返回生效，箭头后的值必须是一个表达式。Javascript 中的表达式在使用时会产生一个值。1 + 1 是一个计算结果为 2 的表达式。语句有点不同，比如`if/else`是一个语句。没有回报的运行之后就没有价值了。使用 if 永远不会允许隐式返回工作。但是三元表达式确实有效，因为它们的结果是一个表达式。

箭头函数还有一个小的遗漏。如果只有一个参数，可以省略括号。拥有 0 或 2+意味着它们是必需的。例外情况是，如果您为参数设置了默认值。

## 隐函数参数

当你创建一个函数时，即使没有定义参数，仍然有一些参数可以在函数中调用。最明显的一个是`this`,它根据被调用的方式而变化。在一个被调用来返回`this`的标准函数中，它将返回全局范围，即浏览器中的`window` 和节点中的`global`。Call、apply 和 bind 都可以改变`this`引用的范围，就像调用它作为对象上的方法一样。

箭头方法也改变了`this`的处理方式。关于`this`如何工作的完整范围有点超出了本文的范围，但是如果你想了解更多，我建议你或者去 [MDN 页面](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this)或者其他人写的大量文章中的一篇。

虽然`this`是相当普遍的知识，但`arguments`就不那么普遍了。在任何函数中`arguments`都是存在的，允许你调用数组中的参数。即使没有任何东西被声明为参数，这也是有效的。例如:

```
function arguementTest () {
  return arguments.length
}arguementTest(0,1,2,3) // this will return 4
```

假设这是一个数组，也可以通过数字调用参数。在上面的例子中，如果我调用了`arguments[1]`,我会得到一个 1 作为第二个值。应该注意，它并不是真正的数组。它可以像一个一样被访问，并且有一个长度属性，但是`forEach()`或`pop()`不起作用。运行`typeof`将显示`arguments` 是一个对象。也不可能在箭头功能中访问`arguments`。

MDN 指出，您实际上不应该在 ES2015(ES6)+代码中使用它。相反，你应该更喜欢所谓的`rest parameter`。这基本上是一个扩展运算符，作为最后一个参数添加，如下所示:

```
function restOp ( one, two, ...rest){
  return rest
}restOp (0,1,2,3,4) // this will return [2,3,4]
```

它的优点是不必考虑命名参数，并且实际上是一个包含所有完整数组方法的数组。rest 参数可以随意命名，但必须始终是最后指定的参数。

## 结尾部分

希望这能启发你一些你可以从 JS 中忽略的东西，或者那些你从未意识到的东西。像往常一样，如果我错过了什么或做错了什么，给我发消息。如果我发现了新的东西或者发现我已经忘记了，我会试着更新这篇文章。