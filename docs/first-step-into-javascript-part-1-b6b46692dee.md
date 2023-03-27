# JavaScript 的第一步—第 1 部分

> 原文：<https://medium.com/swlh/first-step-into-javascript-part-1-b6b46692dee>

让我们进入正题，从总体上理解编程的基本概念。

![](img/955351bcc5df4b9ee1118ddf80053005.png)

Photo by [Fabian Grohs](https://unsplash.com/photos/XMFZqrGyV-Q?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/programming?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

学习如何编程最好的方法是通过实践、实践和大量的实践。所以注意一切，途中编码，创造一个愉快的环境，舒服地坐着，开始学习。

在这篇文章中，我们将讲述编程的基础，当然，我们特别倾向于 JavaScript。在这里，我们将学习高级编程的基本概念。

## 代码:

通常被称为**源代码**或**代码**的程序由一组告诉计算机执行特定任务的语句和指令组成。有效格式或指令组合的规则称为编程语言。有时，称为**语法**。

## 声明:

在计算机语言中，组合并执行特定任务的一组单词、数字和运算符称为**语句**。如果需要，一个程序可以有一个或多个多重语句。这种说法的一些例子是，

```
a = b * 2;
```

在上面的例子中，a 和 b 被称为变量，就像一个存储值的盒子。*和=是运算符，2 是数字和值本身。

JavaScript 中的大部分语句都会以(；)在最后。

## 表情:

语句由一个或多个表达式组成，一个表达式是对一个值或变量或多个值或多个变量的引用，在语句中带有运算符。

```
consider the above example
a = b * 2;
```

这里，

a & b 是**变量表达式**

b * 2 是**算术表达式**

= is **赋值表达式**

2 是一个**文字表达式**

## 执行程序:

像`a = b * 2`这样的语句在读写时对开发人员很有帮助，但实际上并不是一种计算机可以直接理解的形式。因此，计算机上的一个特殊工具(要么是解释器，要么是编译器)被用来将你写的代码翻译成计算机能理解的命令。

对于一些计算机语言来说，每次程序运行时，这种命令的翻译通常是自上而下，逐行进行的，这通常被称为 ***解释*** 代码。

对于其他语言来说，翻译是提前完成的，叫做 ***编译*** 代码，所以当程序*运行*以后，运行的其实是已经编译好准备就绪的计算机指令。

人们通常断言 JavaScript 是被 ***解释的*** ，因为 JavaScript 源代码在每次运行时都会被处理。但这并不完全准确。JavaScript 引擎实际上 ***编译*** 运行**上的程序**然后立即运行编译后的代码。

```
a = 21;                          //21
b = a * 2;                       // 21 * 2 = 42
console.log(b);                  // The output is 42
```

## 输出:

输出通常意味着显示结果，最常见的是

```
console.log();
```

让我们把上面的代码分成两部分，控制台和日志()。其中 log()是函数调用，控制台是函数所在的对象引用。另一个最常见的是

```
alert();
```

## 输入:

最常见的输入方式是 HTML 表单或任何其他确认或点击发生。像 console.log()一样，有一个特殊的输入，它是

```
check = prompt("What is your favourite hobby?");     //input
console.log(check);                                  //output
```

## 操作员:

运算符是我们对变量和值执行操作的方式。我们已经看到了两个 JavaScript 操作符，`=`和`*`。

`=`等于运算符用于*赋值*——我们首先计算`=`右边*的值*(源值)，然后将其放入我们在*左边*(目标变量)指定的变量中。

```
a = 10;                    // The value in a variable is 10 
b = a + 20;                // The value in variable b is 10+20 = 30
console.log(b);            // The output: b = 30 ,printed in console
```

一些操作符是，

**算术运算符:**加法(+)、减法(-)、乘法(*)和除法(/)。这些是算术运算符。**运算符优先级**从上到下依次是乘法、除法、加法和减法。

**加减运算符:**反复加减时使用。这些通常用于**循环**中。

```
a++, a — , — a, ++a
```

**赋值运算符:**赋值运算符是给变量赋值的运算符。我们已经多次使用了最基本的一个函数`=`——它只是将左边的变量赋值给右边的变量

```
a =  10;     // = are the assignment operator
a += 10;     // Which means  a = a + 10; 
a -= 10; 
b *= 10;
```

**比较运算符:**有时我们会想要运行真/假测试，然后根据测试结果采取相应的行动——为此我们使用**比较运算符**。

```
20 === 10+10;   //Strictly equals to
20 !== 10;      //Strictly not equals to
10 < 20;        //Less than
10 <= 10;       //Less than or equal to
```

这个太牛逼了，可以参考 [**Mozilla 开发者组织**](https://developer.mozilla.org/bm/docs/Web/JavaScript) 进一步阅读 **JavaScript** 中的编程基础知识。在接下来的博客中，我们可以深入探讨**值&类型**、**变量**、**块**、**条件**、**循环**等等。

直到那时快乐编码！

```
Practice, Practice and Practice is the only way to learn.Consistency is the key. Never lose Hope!!!
```

## 如果你喜欢这篇文章，别忘了在结尾鼓掌。谢谢你的时间和努力。下一篇博客再见。

*我是* [***毗湿奴***](https://twitter.com/ArchitectUX) *，一名自学成才的开发者/设计师，在一家名为*[***customer labs***](http://www.customerlabs.co)*的产品开发创业公司工作。我们致力于通过让数字营销人员的生活变得简单来创建数字营销基础设施。如果你是一个数字营销者，那么请在这里查看我们的*[](http://www.customerlabs.co)**。**

*[![](img/308a8d84fb9b2fab43d66c117fcc4bb4.png)](https://medium.com/swlh)*

## *这篇文章发表在 [The Startup](https://medium.com/swlh) 上，这是 Medium 最大的创业刊物，拥有+406，714 名读者。*

## *在这里订阅接收[我们的头条新闻](http://growthsupply.com/the-startup-newsletter/)。*

*[![](img/b0164736ea17a63403e660de5dedf91a.png)](https://medium.com/swlh)*