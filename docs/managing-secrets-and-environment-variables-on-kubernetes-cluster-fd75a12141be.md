# 在 kubernetes 集群上管理秘密和环境变量

> 原文：<https://medium.com/swlh/managing-secrets-and-environment-variables-on-kubernetes-cluster-fd75a12141be>

本文主要关注使用 Bitnami-labs 的`kubeseal`项目来管理 Kubernetes 集群上的应用程序的秘密。

感谢 Bitnami 的工作人员制作了这个[项目](https://github.com/bitnami-labs/sealed-secrets)。我个人觉得这对于用 GitOps 的方式管理秘密很有帮助。秘密是单向密封的，因此您可以安全地将它们存储在您的 GitHub 存储库中。我写了一个小的命令行工具，可以轻松地将你的秘密封存起来，并在 Github 的储存库中查看。我们将一步一步来，所以让我们开始吧！