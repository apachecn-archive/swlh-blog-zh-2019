# AWS 中转网关—非对称路由、共享服务 VPC 及其他

> 原文：<https://medium.com/swlh/aws-transit-gateway-asymmetric-routing-and-shared-services-vpc-9d4a4a5581e8>

在本文中，我想讨论 Transit Gateway 和 Route53 Resolver 为多 VPC 环境带来的新的可能性。我们将从多 VPC 体系结构的小例子开始，并将其扩展到共享 NAT 服务和共享 VPC 接口端点。这个例子可以很容易地扩展或适应许多变化和要求。

我们将从定义我们的 VPC 的需求开始。假设我们想拥有一组**私有 VPC**和**中转 VPC** (事实上它更像是“共享服务”VPC，但我…