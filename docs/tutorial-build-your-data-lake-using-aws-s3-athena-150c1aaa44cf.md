# 教程:使用 AWS S3 和雅典娜构建您的数据湖

> 原文：<https://medium.com/swlh/tutorial-build-your-data-lake-using-aws-s3-athena-150c1aaa44cf>

![](img/c27d25388b372a6f692a9871c44db99a.png)

**问题**:我们正在寻找一种方法来提高数据分析师的工作效率，因为 redshift 没有承受压力(我们总共拥有约 8B 条记录)，我们希望降低拥有一个巨大的 Redshift 集群(目前拥有 32 个节点)的成本。

**红移低效**:红移中的每个节点都拥有两个处理(cpu/io) &磁盘空间——它们是…