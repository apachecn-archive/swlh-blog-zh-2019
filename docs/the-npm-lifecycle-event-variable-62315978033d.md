# NPM 生命周期事件变量

> 原文：<https://medium.com/swlh/the-npm-lifecycle-event-variable-62315978033d>

![](img/11e30840d23553134d602b3e42a5fed2.png)

> `**npm_lifecycle_event**`环境变量被设置为正在执行的周期的任何阶段。因此，您可以将一个脚本用于流程的不同部分，该脚本根据当前发生的情况进行切换。

对象是按照这种格式展平的，所以如果您的 package.json 中有`**{"scripts":{"install":"foo.js"}}**`,那么您会在脚本中看到: