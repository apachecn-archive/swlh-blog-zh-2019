# 指标和 KPI

> 原文：<https://medium.com/swlh/metrics-and-kpis-4b8ca30b3d09>

## 来自“软件工程食谱”系列——如何通过客观的观察来提高团队的绩效

我带来了“软件工程食谱”系列的另一篇文章，目标是成长中的工程团队。这一次，我们将讨论这个令人兴奋的话题(对吗？)的绩效衡量。我们走吧！

![](img/875caddd80ea129278d46800e676fb13.png)

Photo by [NESA by Makers](https://unsplash.com/@nesabymakers?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## **尤尼科恩公司——案例研究(受真实事件启发)**

我们在做什么？我们在朝着正确的方向前进吗？看起来积压的工作总是比我们能做的多，而且还在继续增长！

我们过去常常在晚上 8 点结束工作，后来变成了晚上 8 点 30 分，然后是晚上 9 点。在你知道它之前，我们在周五晚上的会议上一直持续到晚上 11 点，在一个好的冲刺阶段持续到凌晨 3 点。

我们的代码真的那么糟糕吗？我们是不是贪多嚼不烂？等等，为什么有两个小孩在家等着他的约翰不再回去和家人一起吃饭了？

这就是 Unikorn 的生活，这是一家雄心勃勃的初创公司，随着计算世界变得越来越移动，它以极快的速度生产软件。

该团队非常有才华，由一位非常聪明和熟练的软件工程师 CTO 领导，但他们缺乏一种方法来将这种原始能量集中到高效的软件生产中，导致正确估计工作的能力降低，因此经常以关键时间结束。

部分问题是缺乏一个简单的方法来客观地衡量进展，并确定薄弱环节。

![](img/0e0f4fd884f24f7bd44fb5ab647f4e31.png)

Photo by [Stephen Dawson](https://unsplash.com/@srd844?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## **测量性能**

从表面上看，检查团队的表现非常简单。你可以看到你的软件在冲刺阶段有多好，有多稳定。如果一切顺利，你可以召集一次会议，拍拍每个人的背，然后继续前进。对吗？

不完全是。“好”和“稳定”对不同的人有不同的含义，因为它们是主观的概念，就像“漂亮”和“有用”一样。

为了客观地衡量绩效，首先你要建立一些客观的(不受制于某人的解释，或“直觉”)和可以用数字衡量的指标。一些很好的例子是:

*   发布后第一周报告的错误数量
*   花在修复错误和开发新功能上的时间
*   每 1000 次 API 调用中的错误数

您可以逐个 sprint 地跟踪这些指标，并分析团队的绩效如何随着时间的推移而进步。对于冲刺阶段结束的会议，他们也是一个很好的对话开始，会议的主要目标是从冲刺阶段发生的具体事件中学习，以避免历史重演。

为每个指标设定目标，并获得团队的认同以实现这些目标也是有用的。这是团队提高绩效的重点，也是绩效评估期间的一个好话题。

一个示例目标是在 sprint 中花费少于 25%的时间来修复 bug，这可以通过更好的质量实践(减少每次 sprint 引入的问题数量)来实现。

![](img/3e79bdfbfc7200d8406a042d366fe1b1.png)

Photo by [Andreas Klassen](https://unsplash.com/@schmaendels?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## **关键绩效指标**

如果你坐下来思考你的团队的所有活动，你可能会想出十几个这样的度量标准，有些直接与每个工程师相关(例如，每个故事点的平均小时数)，有些更多是团队范围的(例如，每个 sprint 的 bug 数)。

这很好，有助于识别团队中的问题，但它们不能显示团队在公司战略目标下的整体表现。

这就是关键绩效指标(KPI)的用武之地。

KPI 是可以与这些战略目标直接关联的(高级)指标，它们主要有两个用途:

*   **上升**:他们给首席执行官/董事会/执行团队一个想法，告诉他们这个团队如何帮助公司实现目标
*   **下**:他们以一种更贴近日常工作的方式向团队传达公司的目标

一般来说，有两种方法可以用来得出一组 KPI:

*   **挑出**:选择你的团队中最重要的指标，在这些指标中，被衡量的职能和公司目标之间有很好的匹配
*   **汇总**:将几个指标合并成一个 KPI

单出方法产生的 KPI 更容易产生，但可能无法提供团队绩效的全貌，而汇总方法以更复杂的评分系统为代价提供了更好的“覆盖范围”。

每个 KPI 都应该有一个解释键，以帮助读者明确地确定团队做得有多好。例如，如果你选择“发布后第一周报告的 bug 数量”作为 KPI，4 是好是坏？

我的建议是构建 KPI，使它们产生一个介于 0 和 5 之间的分数，其中 0 表示“差”，5 表示“优秀”，尽管这在您的组织中可能会有所不同，因为您可能会从高管团队的其他成员那里获得公司范围内记分卡的指导方针。对于“bug 数量…”指标，评分系统可能如下所示:

*   10 个以上的 bug > >**0 分**
*   5–9 个 bug > >**1 分**
*   3–4 个 bug > >**2 分**
*   2 bug > >**3 分**
*   1 bug >> **4 分**
*   没有 bug > >**5 分**

此外，对于度量标准，定义和传达每个 KPI 的目标分数以帮助团队实现其绩效目标是很重要的。在上面的例子中，也许 3 分是一个合理的目标。

![](img/39cd272eef1c8ca06d2e92d125a0cb93.png)

Photo by [Alex Radelich](https://unsplash.com/@alexradelich?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 做工作

可能需要一些时间来制定一套准确反映团队绩效的指标和相关 KPI，但这是一项值得努力的工作，因为它为您提供了一个团队可以集中精力解决的可行项目列表。

我的建议是尝试一些不同的指标，看看什么适合您的特定场景。我曾经为我领导的一个团队定义了 7 个 KPI，结果只是定期跟踪 4 个，因为这些是真正的“关键”KPI。其他三个很稳定，波动很小，所以他们最终放弃了。

就执行而言，您最初可能希望手动整理这些 KPI。这适用于较小的团队，但是很快就变得耗时并且越来越容易出错。

更好的方法是使用更容易收集指标的工具，并基于这些工具中可用的数据定义指标。例如:

*   如果您使用 JIRA 或 Pivotal 进行任务跟踪，您可以收集状态转换的时间戳，以确定在特定故事上花费的时间
*   您可以使用您的源代码控制系统的日志来确定在一个给定的 sprint / branch 中有多少提交是 bug 修复和新特性
*   如果您有一个代码审查系统，您可以很容易地确定每次提交的平均修订次数

多年来，我构建了许多脚本(使用 NodeJS 的最新版本),这些脚本可以从这些不同的系统中提取数据，并将它们转换成 CSV 格式。然后，我将这个 CSV 导入到一个电子表格中(Google Sheets，但 Excel 也可以)，该电子表格使用数据透视表来整理数据并生成指标和 KPI 报告(有一些手动干预，主要是围绕格式)。

对于一个 20/30 人的团队，我每周要花大约 1 个小时来生成这些报告及其相关分析(浏览数据并提取感兴趣的主题，以便与我的开发经理和执行团队进行讨论)。

![](img/99e0113fc65b021f050a1b2b14b86355.png)

Photo by [Djim Loic](https://unsplash.com/@loic?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

绩效管理并不是最迷人的话题，因此我很感激你坚持到这篇文章的结尾。

我希望您喜欢阅读它，尤其是如果您是第一次设置系统来收集指标，或者如果您刚刚被要求为您的团队提出一组 KPI。

在我们开始之前，我想分享一个简短的示例指标列表，您可能会发现这是一个有用的起点，可以用来定义您自己的指标，按功能/方面分类:

*   **功能质量**:在开发的每个阶段(实现、集成、QA、用户验收测试)发现的缺陷、缺陷与特性、缺陷的数量
*   **代码质量**:每次提交的修改数量，每次冲刺的重构故事数量
*   **速度**:每天故事数/ sprint /项目/工程师，每天点数/ sprint /项目/工程师
*   **文档**:在实施过程中对规范的修改数量(由于缺乏细节)
*   **操作**:计划停机、非计划停机、发布引起的停机

像往常一样，请随意分享您的经验，并在下面留下您对软件工程团队的性能度量主题的想法，我将非常乐意加入讨论。

同时，祝你一路顺风，继续做好事！