# 珠子手镯——珠子数据结构的对象序列化策略

> 原文：<https://medium.com/swlh/beads-bracelet-an-object-serialisation-strategy-for-beads-data-structure-fa3732a42211>

Beads 数据结构是一种仅附加的异构数据结构，它直接在附加上应用位打包技术。这是一个简单的值序列的序列化策略。当一个值被追加时，它被检查，最小的表示被存储在缓冲区中。

为了区分缓冲区中的异类值，我们存储了一个 4 位标记来标识值类型。由于标签只有 4 位，我们将它们成对存储在 1 个字节中。