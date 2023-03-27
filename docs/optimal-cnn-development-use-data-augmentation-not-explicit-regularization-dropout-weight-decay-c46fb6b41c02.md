# 最佳 CNN 开发:使用数据扩充，而不是显式正则化(丢失、权重衰减)

> 原文：<https://medium.com/swlh/optimal-cnn-development-use-data-augmentation-not-explicit-regularization-dropout-weight-decay-c46fb6b41c02>

辍学，体重下降和数据增加都已成为每一个 CNN 开发人员的标准工具包的一部分。假设每个都以有希望的协同方式对产生最佳 CNN 做出贡献。然而，加西亚-柯尼希([https://arxiv.org/abs/1806.03852v4](https://arxiv.org/abs/1806.03852v4))最近的研究探索了这一假设，并发现一个更好的开发方法是完全依靠数据增强来产生自我调整的 CNN，并放弃使用辍学和…