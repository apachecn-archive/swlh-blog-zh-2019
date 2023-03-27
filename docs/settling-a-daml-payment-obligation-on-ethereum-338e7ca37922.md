# 结算以太坊上的 DAML 支付义务

> 原文：<https://medium.com/swlh/settling-a-daml-payment-obligation-on-ethereum-338e7ca37922>

[DAML](https://daml.com/) 是一种很棒的开源语言，用于建模多方之间的权利和义务。

双方之间的 DAML 协议可能要求债务人采取行动，如提供健康治疗、粉刷围栏或向受益人转移一定金额的资金。这种义务方便地在 DAML 智能合约的`agreement`字段中表达，并在分类账外结算。

提供医疗服务或粉刷某人的围墙意味着与物质世界互动，因此这些义务*必须* …