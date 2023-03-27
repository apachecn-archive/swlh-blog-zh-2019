# 分支预测—您需要知道的一切。

> 原文：<https://medium.com/swlh/branch-prediction-everything-you-need-to-know-da13ce05787e>

在计算机科学中，**预测**是一种体系结构特征，它为控制的条件转移提供了一种替代方案，由机器指令实现，如条件转移、条件调用、条件返回和转移表。预测通过执行来自分支的两条路径的指令来工作，并且只允许来自被采用的路径的那些指令来修改架构状态。[【1】](https://www.cs.nmsu.edu/~rvinyard/itanium/predication.htm)

为了更好的理解这个概念，我们举个例子:
*“处理一个排序的数组比处理一个未排序的数组快”*