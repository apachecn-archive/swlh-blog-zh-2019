# 如何回答数据科学面试编码问题？

> 原文：<https://medium.com/swlh/how-to-answer-data-science-interview-coding-questions-b5e6b2335c7e>

## LinkedIn 上目前有超过 1829 个开放的机器学习工程职位。

看看过去几年雇用数据科学家的趋势，越来越明显的是，数据科学家不仅分析数据和建立模型，而且他们还编程以大规模部署算法。数据工程是数据科学家日常生活的一个重要方面。作为一名数据科学家，今天每个人都在寻找涉及相当多编码的机器学习专业知识。在[担任 AI](https://medium.com/acing-ai) 时，我一直在努力帮助数据科学家进入数据科学角色。如今，所有招聘这个职位的科技公司通常都会从编码测试开始。本文旨在提供一种方法来回答数据科学面试或编码测试中提出的编码问题。

面试官提出了一个问题，想看看你是如何解决的。解决问题的能力也在这里受到考验。受访者寻找问题解决方案的方法与解决方案本身一样重要。因此，直接跳到最佳解决方案可能不是最好的方法。让我们通过一步一步的解决方案来理解回答编码问题的方法。下面的问题以前在许多数据科学编码访谈中被问过。

![](img/558686e211f5738b06a35c236842316b.png)

Photo by [Adi Goldstein](https://unsplash.com/photos/mDinBvq1Sfg?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/code?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

***给定一个整数数组，返回两个数的索引，使它们加起来达到一个特定的目标。***

你可以假设每个输入只有一个解，你不能两次使用同一个元素。

例如:给定 nums = [2，7，11，15]，target = 18，

nums[1] + nums[2] = 7+ 11= 18，return [1，2]。

回答编码问题的一个好方法是从一个强力解决方案开始，它给出了一个非优化的性能。在面试过程中，面试官希望受访者通过思考其他解决方案来优化解决方案。

让我们采取同样的方法来寻找解决办法。

**暴力破解方法:**

强力方法是有两个 For 循环。第一个 For 循环是从数组的第一个元素循环到最后一个元素，而第二个 For 循环是从数组的第二个元素循环到最后一个元素。然后我们试着从目标中减去这些元素来找到它们。

```
public int[] twoSumBruteForce(int[] numbers, int target) {
   for (int i = 0; i < numbers.length; i++) {
     for (int j = i + 1; j < numbers.length; j++) {
        if (numbers[j] == target — numbers[i]) {
           return new int[] { i, j };
        }
     }
   }
}
```

复杂性分析

*   时间复杂度:O(n)。对于每个元素，我们试图通过遍历数组的其余部分来找到它的补码，这需要 O(n)时间。因此，时间复杂度为 O(n)。
*   空间复杂度:O(1)。

**优化:**

我们的时间复杂度需要改进。在这种情况下，时间复杂度很低，因为我们遍历了两次循环。此时，我们应该考虑减少元素查找的数据结构。让我们考虑使用散列表作为在散列表 O(1)中查找大部分元素的时间。如果有冲突，查找时间将达到 O(n ),但是只要仔细选择散列函数，对散列表的查找应该分摊到 O(1)时间。

使用哈希表的一种方法是将元素存储到由数组索引索引的哈希表**中。然后使用 For 循环，我们可以查找每个元素的补码(目标`-`元素值)是否存在。这将是 O(n ),因为我们遍历元素两次，第一次是在将它们添加到表中时，第二次是在获取补数时。这将需要两遍。让我们进一步优化只有一个通道。我们可以迭代并将元素插入到表中，同时还可以回过头来检查当前元素的补码是否已经存在于表中。如果存在，我们返回值。**

```
public int[] twoSum(int[] numbers, int target) {
     Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < numbers.length; i++) {
            int complement = target — numbers[i];
        if (map.containsKey(complement)) {
            return new int[] { map.get(complement), i };
         }
     map.put(numbers[i], i);
   }
}
```

复杂性分析:

*   时间复杂度:O(n)。我们只遍历包含 n 个元素的列表一次。表格中的每次查找仅花费 O(1)时间。
*   空间复杂度:O(n)。所需的额外空间取决于哈希表中存储的项目数量，哈希表最多存储 n 个元素。

从这个例子中，我们看到我们将时间复杂度从 O(n)交换到 O(n ),将空间复杂度从 O(1)增加到 O(n)。对于在数据上执行的任何算法，必须考虑这样的折衷。在数据科学领域，完全由构建模型的人来优化 GPU 和时间。GPU 具有与之相关的成本价值。因此，重要的是要有一个最佳的时间成本比。在大多数情况下，时间总是优化的，但也有一些情况下，我们不能拥有无限的 GPU，或者添加更多的 GPU 不会大幅减少时间。一个谨慎的数据科学家总是会看到什么是可用的，并在时间和成本之间进行权衡。

*感谢阅读！*😊*如果你喜欢，测试一下你能打多少次*👏*再过 5 秒。这对你的手指来说是很好的有氧运动，并且会帮助其他人看到这个故事。*

[](https://medium.com/acing-ai) [## 阿信艾

### 《人工智能》提供人工智能公司的分析以及进入这些公司的途径。

medium.com](https://medium.com/acing-ai) 

*这篇博客文章的唯一动机是为一些数据科学面试问题提供答案。我的目标是使这成为一个活的文档，所以任何更新和建议的变化都可以被包括在内。请提供相关反馈。*

[![](img/308a8d84fb9b2fab43d66c117fcc4bb4.png)](https://medium.com/swlh)

## 这篇文章发表在 [The Startup](https://medium.com/swlh) 上，这是 Medium 最大的创业刊物，拥有+412，714 名读者。

## 在这里订阅接收[我们的头条新闻](http://growthsupply.com/the-startup-newsletter/)。

[![](img/b0164736ea17a63403e660de5dedf91a.png)](https://medium.com/swlh)