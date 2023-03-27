# D3 动画条形图

> 原文：<https://medium.com/swlh/d3-animated-bar-chart-ede2a10709b>

为了创建条形图，你应该对 javascript 和 d3 有所了解。

D3 是一个 javascript 库，用于创建数据的可视化。它可以处理许多类型的数据格式。例如 JSON、CSV、XML、TSV 等等。它提供了不同类型的图表，如酒吧，饼图，线等。你可以在 d3 的官方网站或[这里](https://github.com/d3/d3/wiki)了解更多关于 D3 的信息。

为了创建条形图，我们需要数据([虚拟数据](https://www.generatedata.com/))。我的数据是 JSON 格式的。它告诉我们在某个文本中字母使用了多少时间。

## `[ {
"Letter": "A",
"Freq": 20` …