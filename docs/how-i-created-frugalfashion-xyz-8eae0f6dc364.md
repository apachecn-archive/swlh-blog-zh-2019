# 💸我如何创建 frugalfashion.xyz

> 原文：<https://medium.com/swlh/how-i-created-frugalfashion-xyz-8eae0f6dc364>

我注意到我的几个朋友虔诚地查看 subreddit[r/frugalmalfashion](https://reddit.com/r/frugalmalefashion)上的交易，特别是[的普通项目](https://en.wikipedia.org/wiki/Common_Projects)下降。在 2018 年假期期间，我创建了 [frugalfashion.xyz](https://frugalfashion.xyz) ，这是一个跟踪[r/frugalfalefashion](https://www.reddit.com/r/frugalmalefashion)和[r/frugalfalefashion](https://www.reddit.com/r/frugalfemalefashion)的网络应用程序。用户通过关键词指定他们感兴趣的产品，然后应用程序使用它们过滤子关键词。这些过滤器用于发送相关帖子的电子邮件通知，并通过 [tweets](https://twitter.com/rfrugalfashion) 将它们发送出去！那么我是怎么做到的呢？

![](img/1d3a7c1d495a1a802f50de1c4621a15a.png)

**Original design**

## 概观

1.  **Cron 脚本**民意调查 reddit
2.  Mysql 存储新帖子
3.  **SQL 插入触发下游作业** —电子邮件和 twitter 通知
4.  **CRUD 服务器** —提供用户界面，从 mysql 获取帖子
5.  **Firebase** —验证并存储用户元数据

事实证明 firebases 的免费层对于每个用户的几个关键字和切换来说有足够的空间。唯一隐现的限制是支持并发 http 连接，它被限制为 100 个。在 [frugalfashion.xyz](https://frugalfashion.xyz) 的例子中，平均并发连接数是 25，所以现在一切正常🤞

![](img/edd2ac1f612a348a58c8448b15a22c36.png)

**Finalized design**

## 投票 Reddit

crontab 是我定期投票给 reddit 的首选武器。我选择 cron 轮询是因为它的守护进程性质，独立于 web 应用程序。总体愿景是一系列可供消费的帖子，无论是 web 应用程序用户、电子邮件通知还是 twitter 通知。将轮询逻辑从这些服务中分离出来是必要的，cron 守护进程完美地体现了这种行为。使用 crons 的唯一缺点是分钟是最低级别的时间粒度，限制了每分钟一次的轮询频率。缓解时间限制的替代解决方案是[循环调用](https://twistedmatrix.com/documents/9.0.0/api/twisted.internet.task.LoopingCall.html)或休眠的无限循环，类似于 [python 轮询](https://github.com/justiniso/polling/blob/master/polling.py#L100)的实现。

## 存储帖子

mysql 是我的首选数据库，因为它简单、具有 ACID 属性，并且有丰富的 binlog 处理软件。binlog 是 mysql 在每次数据库发生变化时写入的日志文件。我想通过 kafka 作为实时通知的代理来试验被动的日志消耗。或者，我可以直接从 cron 脚本发送通知。然而，更多的逻辑将被添加到 cron 中，它有一分钟长的时间限制，我将有一个更大的脚本，这很无聊😊。

回想起来，一个 [elasticsearch](https://www.elastic.co/products/elasticsearch) 集群在通过关键字搜索帖子和进行更复杂的字符串查询(这是这个应用程序的本质)时会更有性能，但是，对于当前的应用程序状态来说，它是多余的。

## 使用二进制日志

卡夫卡和马克斯韦尔是唯一可靠的久经沙场的老兵。Kafka 是一个等待从生产者那里获取数据的服务器。麦克斯韦是我们卡夫卡的制片人；一个服务器跟踪 mysql binlog 并在 post inserts 上写入 kafka 主题，我们使用它来发送 twitter 和电子邮件通知。我之前提到过，我正在被动地阅读 binlog 来模拟实时通知，这都是通过 maxwell 和 kafka 提供的生产者/消费者模型来完成的。我可以廉价地创建另一个 kafka 消费者来索引一个弹性搜索集群，以进行更高性能的关键字搜索。

## 通知

Twitter 和电子邮件通知是作为 kafka 消费者单独设置的。对于 twitter，tweets 是在每个具有适当标签的插入页上创建的。对于电子邮件，遵循类似的过程，但需要注意的是，会进行 firebase 查询，以确定哪些用户想要所有通知的电子邮件，或者只想要包含其指定关键字的特定通知的电子邮件。我目前使用 [mailgun](https://www.mailgun.com/) 作为我的专用 smtp 服务器，最初并不太好，因为免费层服务器的[发送分数](https://help.returnpath.com/hc/en-us/articles/220569187-What-is-Sender-Score-and-how-is-it-calculated-)很低。升级后，电子邮件停止反弹，并成功发送。

## CRUD 应用

我使用 [tornado](https://www.tornadoweb.org/en/stable/) 是因为我❤ Python，tornado 的配置非常简单。这个 web 应用程序是轻量级的，只服务于 UI 和查询 mysql 中的帖子。

## 漂亮的用户界面

作为一个极简主义者，我喜欢大而无重的 UI 元素，我使用了 Pinterest 的 UI 框架[完形](https://pinterest.github.io/gestalt/)！在这个项目之前，我从未使用过格式塔，它使用起来非常简单，尤其是与谷歌的 [material ui](https://material-ui.com/) 相比。当 React 框架通过组件组合而不是组件配置提供高度可定制性时，我发现了它的价值。这意味着组件本质上是瘦的，只有必要的道具，并且是包含非常基本和通用组件的库的一部分。

注意上面代码片段中的组件是如何通用的，然而每一个都用少量的道具服务于它们的目的。这些组件生成帖子列表，看起来很棒，对显示事件反应良好，不需要大量的道具定制。

## 支持基础设施

我使用[数字海洋](https://www.digitalocean.com/pricing/)五美元的服务器来托管和服务一切。疯狂吧？虽然这并不都是阳光和玫瑰，但我不得不牺牲存储空间来交换，以便为臭名昭著的内存猪[卡夫卡](https://kafka.apache.org/)获得更多可用内存。 [Systemd](https://www.freedesktop.org/software/systemd/man/systemd.html) 是一个进程管理器，支持快速进程启动、重启、终止以及相关的内存使用指标。

## 考虑

在过去，我的应用架构通常是由 web 服务器、nginx 和 mongodb 组成的。这消耗了大量的内存，使微调优化和部署麻烦。例如，对通知系统的更改将需要整个服务器的重建和部署，这是不理想的。

除了性能下降，我还会遭受脆弱的逻辑耦合。这通常会扼杀继续项目的动力，更不用说解决后期制作问题了。

对于这个应用程序，我决定采用面向服务的结构，将功能拆分到由 [systemd](https://www.freedesktop.org/software/systemd/man/systemd.html#) 监控的多个任务中。这让我可以专注于需要注意的特定部分，而不会被系统的其他部分淹没。应用程序的每一部分都非常小和轻量级，鼓励更干净、简洁的代码，我不介意在未来处理这些代码。我在 nginx 后面通过 Systemd 分别部署每个通知服务和 web 应用程序。

未来的下游消费者很容易通过我的 kafka 消费者模板创建，这些模板独立存在，完全独立于现有的服务。如果这是一个单一的 web 应用程序，我将需要通读代码来找到我调用服务的地方，并进行必要的修改。

让我们清楚地知道，这种方法远非完美。与耦合的逻辑和大文件不同，人们必须处理多个服务的协调，并跟踪系统如何相互协作。这是一个不同的问题，对我来说是一个新的问题，我现在很喜欢这个问题。

## 结论

总的来说，我对这个结果很满意🤗。该应用受到了社区的好评，在一周内获得了两个 reddit 帖子的 266 次投票，2K 页面浏览量和 300 次注册。最酷的部分是当一个 redditor [转发我的机器人](https://twitter.com/aarbypls/status/1087867053082640384)通知他一笔交易时。点击[这里](https://www.reddit.com/r/frugalmalefashion/comments/addsmp/get_notifications_filter_posts_here_on/)收听未来的功能更新和发布💪。

***如果你喜欢这篇文章，并想继续讨论，或戳我的大脑关于任何事情，请随时联系@***[***ihsanetwaroo@gmail.com***](mailto:ihsanetwaroo@gmail.com)

[![](img/308a8d84fb9b2fab43d66c117fcc4bb4.png)](https://medium.com/swlh)

## 这篇文章发表在 [The Startup](https://medium.com/swlh) 上，这是 Medium 最大的创业刊物，拥有+418，678 名读者。

## 在这里订阅接收[我们的头条新闻](http://growthsupply.com/the-startup-newsletter/)。

[![](img/b0164736ea17a63403e660de5dedf91a.png)](https://medium.com/swlh)