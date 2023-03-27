# 在节点 JS 中设置 ELK 记录器和警报系统

> 原文：<https://medium.com/swlh/setting-up-elk-logger-and-alert-system-in-node-js-cd73f51548e6>

经过几个小时的研究，我找不到一篇包含所有日志设置的独立文章。[这](https://dzone.com/articles/build-your-own-error-monitoring-tool)是一篇相当好的文章，然而它仍然没有向你展示它所使用的软件包中的所有配置。所以，在经历了喧嚣之后，我想我不妨就这个话题写一篇文章。

在进入该过程之前，让我们简单地谈论一下我们将使用的软件包。最棒的是，它们都是免费的。

*   这是一个数据发送器。它是我们日志文件的监视者。如果有的话…