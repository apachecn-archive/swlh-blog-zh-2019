# 我将一个 SQL 查询从 24 分钟缩短到了 2 秒——这是一个查询优化的故事

> 原文：<https://medium.com/swlh/i-modified-an-sql-query-from-24-mins-down-to-2-seconds-a-tale-of-query-optimization-bcf49d50174b>

![](img/9f0bc6e5fa765a2292070684f2821597.png)

去年 12 月，我从 [VWO](https://vwo.com) 支持团队那里得到了一份有趣的 bug 报告。对于大型企业客户来说，特定分析报告的加载时间似乎慢得离谱。因为我是数据平台的一部分，所以立即出现了一个标志，我参与了问题的调试。