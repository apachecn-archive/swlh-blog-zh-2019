# Python 中的 Testcontainers 测试 Docker 相关的 Python 应用程序

> 原文：<https://medium.com/swlh/testcontainers-in-python-testing-docker-dependent-python-apps-bd34935f55b5>

![](img/3841503fc20a6a4d2a29b0100182e56e.png)

“Python wrestling with the **docker-compose squid**”, by the author.

Python 包 [testcontainers](https://github.com/testcontainers/testcontainers-python) 解决了 Python-apps 常见的两个问题。我们开发基于 Python 的应用程序，并使用 [AWS ECS-CLI](https://github.com/aws/amazon-ecs-cli) 进行部署。所以我们直接将一个 **docker-compose** 配置部署到 *AWS ECS* 中。那个**配置**也想在本地**测试**，除了包 *testcontainers* ，我还没有找到合适的解决方案。