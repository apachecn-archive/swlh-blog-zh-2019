# EKS vs. ECS:在 AWS 上编排容器

> 原文：<https://medium.com/swlh/eks-vs-ecs-orchestrating-containers-on-aws-9d49d3ff7f8d>

AWS 于 2017 年 11 月在 re:Invent 上宣布了 Kubernetes-as-a-Service:Kubernetes(EKS)的弹性容器服务。从昨天开始，EKS 就普遍可用了。在 EKS 出现之前，我讨论过 ECS vs. Kubernetes。因此，我想再做一次尝试，将 EKS 与 ECS 进行比较。

在比较差异之前，让我们先来看看 EKS 和 ECS 的共同点。这两种解决方案都管理分布在一组虚拟机中的容器。管理容器包括:

*   监控和替换故障容器。