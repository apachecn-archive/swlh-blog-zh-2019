# 将 Docker 云堆栈迁移到 Amazon ECS

> 原文：<https://medium.com/swlh/migrate-docker-cloud-stacks-to-amazon-ecs-c4b77195ab50>

# 弹性集装箱服务

这篇博客描述了几种从 Docker 云迁移到 Amazon 弹性容器服务(ECS)集群的方法。ECS 是 AWS 的一个托管容器编排服务，负责管理集群状态，并将容器调度到一个容器实例群中。它公开了一个 API，并附带了一些工具，您可以用这些工具轻松地迁移您的容器化应用程序。

迁移 Docker 云应用的步骤如下: