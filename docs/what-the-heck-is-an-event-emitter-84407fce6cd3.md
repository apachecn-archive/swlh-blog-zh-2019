# 什么是事件发射器？

> 原文：<https://medium.com/swlh/what-the-heck-is-an-event-emitter-84407fce6cd3>

![](img/e5acbd15e7a2fc2b37808769005a501f.png)

Photo by [Tobias Cornille](https://unsplash.com/@tobiasc?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/signal?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

事件是许多不同编程语言的重要组成部分。不过，我在这里将重点关注 JavaScript，特别是 NodeJS 事件。节点的架构主要是异步事件驱动的。

# 什么是事件？

在事件发射器出现之前，如果我们想在代码中使用某些功能(比如在一个进程完成后发送一封电子邮件)，我们必须调用这个函数来直接发送电子邮件。

这带来了一些问题，例如在 web 服务器中，如果我们想要存储数据，然后向用户发送电子邮件，我们必须依赖电子邮件发送功能才能成功向客户端返回正确的 HTTP 响应。

我喜欢用广播的方式来描述一个事件。它可能与您当地的广播电台非常相似。为了收听那个电台，你的汽车收音机被调到一个特定的频率。这意味着你车上的接收器正在过滤掉所有其他频率，以便收听你最喜欢的摇滚电台。摇滚电台正在播放他们正在播放的音乐。他们可以发送他们想要的任何东西，你车上的接收器正在你的扬声器上播放。

事件发射器以同样的方式工作。我们有监听器方法(汽车上的接收器)，我们有事件发射器(无线电台广播)。

## 监听器方法

您的侦听器方法正在侦听某个事件(就像您的汽车收音机在某个频率上一样)。一旦该事件被发出，方法*对该事件做出反应*，从而调用该方法。如果从来没有一个事件符合监听器的标准，那么什么都不会发生，就像在你的汽车里听不到音乐一样。

## 事件发射器

您的事件发射器向任何等待响应的方法发送某个事件。就像在广播电台的广播中，没有人必须听才能实际广播事件，但是如果没有听众，那么什么也不会发生。

# 我们如何利用这一点？

就像前面的电子邮件示例一样，事件发射器可以以多种不同的方式使用。

NodeJS 有一个名为…的内置事件模块。你猜对了…事件。你可以使用这个模块来创建你自己的事件，并进行游戏。这通常是最好的学习方法。

# 示例时间

## 工作事件发射器

```
const events = require(‘events’);const emitter = new events.EventEmitter();emitter.on(‘fire’, () => { console.log(‘Fire!’)});console.log(‘Ready!’);console.log(‘Aim!’);emitter.emit(‘fire’);
```

这段代码:
1。导入事件模块
2。创建一个新的发射器对象
3。为“fire”事件
4 创建监听器。日志`Ready!` `Aim!`
5。发出“火灾”事件
6。我们创建的监听日志`Fire!`

这是预期的正确输出:

```
Ready!
Aim!
Fire!
```

## 非工作事件发射器

您必须确保的一件事是，侦听器是在发出 事件之前 ***创建的。如果不是，那么在事件发出时，没有任何东西在监听它。所以如果我们有下面的代码:***

```
const events = require(‘events’);const emitter = new events.EventEmitter();console.log(‘Ready!’);console.log(‘Aim!’);emitter.emit(‘fire’);emitter.on(‘fire’, () => { console.log(‘Fire!’)});
```

这段代码:
1。导入事件模块
2。创建新的发射器对象
3。日志`Ready!` `Aim!`
4。发出“fire”事件。
5。为“fire”事件创建侦听器

然后我们得到以下输出:

```
Ready!
Aim!
```

# 将参数传递给事件侦听器

另一个对事件发射器至关重要的令人惊叹的特性是将数据传递给监听器的能力。就像电台向我们的收音机发送音乐一样。

要做到这一点，很简单:

```
const events = require('events');const emitter = new events.EventEmitter();emitter.on('fire', (word) => { console.log(word)});emitter.emit('fire', 'Ready!');emitter.emit('fire', 'Aim!');emitter.emit('fire', 'Fire!');
```

此代码块:

1.  导入事件模块
2.  创建新的发射器对象
3.  为接受一个参数的“fire”事件创建侦听器
4.  用参数`Ready!`发出“火灾”事件
5.  我们创建的发射器从发射器获取参数并记录下来。
6.  用`Aim!`和`Fire!`重复步骤 4 和 5

这是预期的正确输出:

```
Ready!
Aim!
Fire!
```

# 其他类型的事件发射器

有许多不同的模块、包、框架等..它们都以不同的方式利用事件发射器。

Angular 以我们上面使用的方式使用事件发射器，它们也使用事件发射器在父组件和子组件之间来回传递触发器。

节点的文件系统模块`fs`在创建流时使用它们。当这些流中发生某个动作时，它们会发出事件。像这样:

```
var fs = require(‘fs’);
var rs = fs.createReadStream(‘./somefile.txt’);
rs.on(‘open’, function () {
 console.log(‘File opened’);
});
```

在上面的代码块中，我们创建一个读取流，然后创建一个事件侦听器来读取流发出的事件。这样，当文件被打开时，我们就可以创建一个侦听器，在它被打开时被调用，而不是永远等待它被打开。

# 结论

事件发射器和监听器对于 NodeJS 开发和许多其他编程语言的开发至关重要。当你有一些函数需要“每当其他事情发生时”执行时，它们非常有用，而不需要该函数完成或甚至为此工作。