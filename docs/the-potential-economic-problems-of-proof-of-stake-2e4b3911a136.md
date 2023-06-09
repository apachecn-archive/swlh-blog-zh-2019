# 股权证明的潜在经济问题

> 原文：<https://medium.com/swlh/the-potential-economic-problems-of-proof-of-stake-2e4b3911a136>

![](img/9bb7ad81b23a31741fda7eb39a115871.png)

Photo by [Gratisography](https://www.pexels.com/@gratisography?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) from [Pexels](https://www.pexels.com/photo/black-and-white-people-bar-men-4417/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

利害关系证明(PoS)协议目前在大多数新的区块链项目和以太坊中非常流行，使用或设想使用它们来代替工作证明(PoW)或其他替代方案，如拜占庭容错(BFT)投票。

至少有四种可能的 PoS 方法。像 EOS、Lisk 和 Tron 这样的一些项目已经转向可以说是最简单的 PoS 解决方案，即委托 PoS (DPoS)，其中一组固定的特权节点在每个时间点验证块。然而，这种类型的 PoS 可以说是最没有意思和前景的，因为相关的[治理](/@Vlad_Zamfir/against-on-chain-governance-a4ceacd040ca)和[集中化](https://www.newsbtc.com/2018/12/04/eos-centralization-woes-return-as-block-producer-offers-money-for-votes/)问题可能会阻止它成为大规模采用的基础。赌注也可以用作混合共识机制的一个元素，例如我们最近讨论的由 Thundercore 开发的机制。与此同时，Algorand 团队正在研究他们所谓的[【纯 PoS】](http://proof-of-news.com/index.php/2019/04/26/deep-dive-into-algorand-part-1-algorands-consensus-mechanism/)，其中拥有网络令牌的每个节点都将能够参与共识。

然而，有理由认为，唯一真正的 PoS 是 Ethereum 2.0 (Serenity)、RChain 和 Casper Labs 等公司采用的保税 PoS (BPoS)方法。这种说法的原因是，在其他三种情况下，赌注，如果真的发生了，在共识中起着从属的作用。只有在 BPoS 案例中，验证者提交的硬币数量倾向于决定他们批准给定块的概率，从而决定他们的收入流。

区块链思想领袖们撰写了许多文章，并就使 BPoS 正常工作的复杂技术问题、确保活性和终结性的可能性、避免频繁分叉等展开了辩论。还广泛涉及了最终安排的[激励设计](/@VitalikButerin/a-proof-of-stake-design-philosophy-506585978d51)问题，行为不端的利益相关者应如何受到惩罚等等。然而，BPoS 似乎提出了远远超出狭隘激励设计的重要经济问题，最近，随着[的](https://messari.io/resource/staking-as-a-service)[出现](https://www.coindesk.com/why-coinbases-move-into-proof-of-stake-matters)，人们开始反思这些问题。

在货币或一般交换手段的历史上(我将使用货币或货币这两个术语)，这种方法潜在地代表了一个革命性的步骤。以前从未直接需要货币单位来保证同类货币单位的转让和持有。当然，一个人需要支付少量费用进行银行转账或维持银行账户，但也可以直接持有或给别人现金。更重要的是，即使货币越来越虚拟化，人们在特定时期如何使用货币与银行服务的总体费用之间也没有紧密的联系，反之亦然。现在解开这个想法可能还不容易，但是这导致的两个潜在的主要问题将有望使它变得清晰

但是首先，简单介绍一下 BPoS 在高层次上是如何工作的。希望验证新块的节点所有者应该向绑定智能合同贡献一定数量的网络令牌，这些令牌将作为勤奋行为的一种保证。如果一个利益相关者参与了一系列定义明确的错误行为中的一个，她的股份将被删除，或者用行业术语来说就是“削减”。作为不将他们的令牌用于其他目的的回报，赌注节点有权获得用他们发布的块创建的块奖励和交易费，就像 PoW 上下文中的矿工一样。

**本·达文波特的零持有均衡论点**

回到 BPoS 使用金钱来保护金钱的方法所产生的问题，第一个重要的问题最近在 Bitgo Ben Davenport 的联合创始人在一篇有见地的[媒体文章](/@bendavenport/a-stake-to-the-heart-57fcd8ec323b)中提出。实际上，达文波特提出了一个看似简单的问题。如果从理论上讲，几乎所有人都可以入股，那么当这意味着他们的持股将被集体奖励稀释时，还会有人持有基于 BPoS 的网络代币吗？进入该系统的唯一新资金来自块奖励，这意味着随着时间的推移，赌注节点将积累越来越多的代币，而非赌注节点将一起持有不变的金额。

达文波特认为，由于代币持有者并不愚蠢，最终，没有人会长期持有网络代币，并且每个不打算立即使用代币进行购买的人都会倾向于将代币押上。然而，在这种情况下，没有人会从赌注中获得任何回报，大宗奖励会造成纯粹的通胀。这意味着一旦这种情况发生，赌注可能会崩溃。

虽然看起来很有力，但这种对 BPoS 的反对至少有两个缺陷。首先，一种广泛使用的交换手段(目前没有一种区块链网络代币是这种手段)从在交换中的使用中获得其价值。这意味着，利益相关者希望从他们的收益中获利的唯一方式是在相关价格反映追逐他们想要购买的商品的货币量增加之前进行购买。这与中央银行创造的新货币的第一个接收者从今天的通货膨胀中获利的方式没有太大的不同，经济学家称之为[坎蒂隆效应](https://fee.org/articles/the-cantillon-effect-because-of-inflation-we-re-financing-the-financiers/)。因此，利益相关者在总供给中的份额至少不会像达文波特认为的那样增长。

第二个问题是不确定性。许多人无法准确预测他们在不久的将来需要花多少钱。同时，如果需要的话，结合的代币不能被迅速地取出，因为结合的全部目的是在一定时期内冻结标桩代币。因此，许多人仍然倾向于持有大量的网络代币，即使理论上他们可以下注。

**货币采用和股份之间的反馈回路**

因此，达文波特反对 BPoS 的理由似乎比看起来要弱，但赌注和维护货币完整性之间的紧密联系带来了另一个潜在挑战。

需要做一些初步的观察。虽然当前大多数经济学家倾向于从均衡的角度考虑经济，但经济现象本质上是动态的。对于一种新生的货币，这意味着我们不能仅仅谈论某种最终的稳定状态。作为这种讨论的一个例子，考虑最近在以太坊 2.0 团队成员中关于赌注和发行的 [Github 讨论](https://github.com/ethereum/eth2.0-specs/pull/971)。很明显，他们关注的是从长远来看应该投资多少，而不是投资的过程。

然而，一种新生的货币将首先被用于结算一小部分交易所，然后，如果它成功了，它将扩展到更多的交易所，以此类推。如果以这种货币进行的交易量增长快于其单位数量的增长，以这种货币计价的价格和汇率就会增长。

在基于 BPoS 的网络令牌的情况下，后果之一是网络将变得越来越吸引攻击者或行为不端的标桩节点。如果总股份的初始金额相对较低，这将意味着需要额外的赌注。但是额外的标桩将减少流通中的代币的数量，如果标桩的代币被冻结一段时间，这将使流通的代币更有价值。这将使得网络对攻击更有吸引力，并且需要更多的令牌来增强保护，等等。

这可能会给新生的基于 BPoS 的网络带来很多不稳定性和不确定性。即使增加的购买力波动证明不是戏剧性的，也不清楚如何才能使频繁增加赌注总额的过程变得平稳。原因是，改变赌注总额将稀释当前债券验证人的股份，从而降低他们的预期回报。这意味着他们可能会抵制赌注总额的增加。

一个潜在的解决方案可能是从一开始就要求在网络令牌供应中占有足够大的部分。然而，要做到这一点可能很难，因为潜在的绑定验证者可能会预计网络令牌的价格会在发布时大幅上涨，并且不愿意承诺太多。或许，解决这个问题的方法是将整体奖励设定在一个足够高的水平。不过，这种方法也有其不利之处，因为它会提高预期通胀率。一旦网络稳定在某个交易量附近，未来是否能做出可信的承诺来减少大宗交易报酬，还有待观察。

***

不管本文中讨论的 BPoS 的潜在挑战是否会被证明是真正重要的，推广 BPoS 将是一个有趣的经济实验。在此之前，一种货币从未被直接用于保护自身参与的持有和结算系统。