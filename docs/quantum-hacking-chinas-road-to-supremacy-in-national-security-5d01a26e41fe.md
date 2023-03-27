# Quantum Hacking: China’s Road to Supremacy in National Security

> 原文：<https://medium.com/swlh/quantum-hacking-chinas-road-to-supremacy-in-national-security-5d01a26e41fe>

Quantum mechanics, computing and cryptography has gained popularity due to the seemingly [odd physics that governs the fundamental particles](/@RichMazzola/a-beginners-guide-to-a-hidden-reality-quantum-mechanics-and-musings-on-a-simulated-reality-e48c20d66b03) of the world. This has led to a sustained, incremental increase in popularity of ‘quantum’ based topics worldwide.

![](img/e7f7bd2be8fc8c0c54b033d807a6f967.png)

Source: Google Trends 2019

However, this increased popularity has led to a dis-proportional increase in click bait or down right factually inaccurate headlines.

![](img/4329fa13ff73b04ff5424b0a1530bd18.png)

These falsified headlines are not usually malevolent, they result from a fundamental illiteracy within science. The United States, which currently ranks 24th and 50th respectively in science and math literacy owns a fair share of the blame ([source](https://www.pewresearch.org/fact-tank/2017/02/15/u-s-students-internationally-math-science/)).

Scientific illiteracy is widespread beyond the general public of the United States. Out of 535 members of the United States Congress (Senate + House of Representatives) only one member holds a PhD in a science related field([source](https://www.pri.org/stories/2017-01-23/only-physicist-congress-state-science-hill)).

Compare this to China where 8 out of the top 9 government officials are scientists and engineers, ([source](https://gineersnow.com/leadership/chinese-government-dominated-scientists-engineers)) including their president Xi Jinping who has a degree in chemical engineering and a PhD in law.

This trend is troubling because the United States apathy towards scientific progress is becoming a National security concern. The US won’t be able to draft off of the progress that the Chinese make in certain technological fields; quantum cryptography being one of the most important.

To understand why quantum cryptography is different than previous technologies, it’s useful to classify history’s technologies into two categories:

1.  **Open door technologies.** Open door technologies spread opportunity from the creator to the masses. An example of an open door technology was penicillin. Once it was created it was spread, development increased and costs decreased over time until the access to this technology became almost ubiquitous.
2.  **闭门技术。**闭门技术的所有价值都被创造者提取，价值集中在一个实体中。闭门技术的一个例子是基于裂变的核弹。一旦这项技术被创造出来，它就不会被分享，它的创造者会提取它所有的“价值”。对于某些闭门技术，没有其他实体会使用它们，因为最初的使用会破坏未来的前景，即引爆的核弹消除了其他人使用它的机会。

谈到量子技术，中国已经认识到这种差异，并致力于赢得量子密码的军备竞赛。

![](img/6f37fdddb1cd9e1529369955592ec542.png)

There an arms race for Quantum supremacy between the US & China.

为了充分理解中国决定的战略性质，调查密码学的现状、量子计算将如何改变这一现状以及这对数字安全的未来有何影响是很重要的。

# 加密概述:

有很多加密技术。为了简单起见，这里的重点是 RSA 加密。这种类型的加密主要基于四项:

*   公开密钥
*   私人密钥
*   因数分解
*   质数

让我们举下面的例子。假设您想加密您和朋友之间的一条消息。您可以通过执行以下操作来加密它:

公钥= 25

私钥= 25(5，5)的正确素数因式分解。

质数是一个整数，它只有两个因子——1 和它本身。在这种情况下，您的接收者将拥有与公共代码(25)匹配的素数(5，5)的私人代码，并且消息将解锁。这将是一个非常糟糕的加密系统，因为只有一种方法可以用质数(5，5)来分解 25，因此任何坏人都可以很快破解这个加密。

现在，假设您选择了一个 100 位数的公钥，并将其称为[x]。黑客会如何破解这段代码？

1.  他们会知道他们只需要达到起始公钥号码[x]的*平方根*。那是因为因式分解中的两个质数不能超过公钥([源](https://stackoverflow.com/questions/5811151/why-do-we-check-up-to-the-square-root-of-a-prime-number-to-determine-if-it-is-pr))的*平方根*，否则相乘在一起就会超过公钥的大小。这将公钥[x]的平方根中包含的位数减少到大约 50 位。让我们打这个号码[y]。
2.  只有一种方法可以确定一个非质数的因数，那就是用它除以所有可能的质数因数。现在，黑客们需要找到所有小于[y]的质数，并尝试将[x]除以他们发现的每个质数。如果这个计算产生了另一个质数，那么他们已经发现了一个潜在的私钥。
3.  对于一个 50 位数的数，比如我们例子中的[y],大约有 1110000000000000000000000000000000000000 个素数因子的可能性。这超过了代表整个地球的原子数量。这意味着，即使你有一台计算机试图在一秒钟内对这些素数进行百万次除法运算，你仍然需要 211100000000000000000000000 年才能得到正确的因式分解来破解这种加密(也就是说，到那时太阳已经燃尽)。*感谢 Scott Welch 的计算。*

需要注意的是，质数因式分解并不难。你很快就能算出 25。问题是你需要用同样的过程来分解 25，就像你分解一个 50 位数的数一样，也就是一次分解一个质数分解集。

问题是没有办法*有效地*找到一个非质数(称为合数)的质数因子。这一事实就是加密在通信中变得无处不在的原因——这也是我们将会回归的事实。

# 量子密码术:

我们已经确定信息传输是安全的(即不能被解密)，因为进行质数因式分解需要非常长的时间。但是量子计算机改变了这个问题的两个重要方面:

1.  你不再需要一次尝试因子 1
2.  你可以更快地分解数字

这是可能的，因为量子计算机不像普通计算机那样工作。普通计算机通过二进制位(1，0)处理信息。量子计算机能够通过使用可以同时以多种状态存在的量子比特(或 quibits)来更快地处理信息。利用这项技术，[科学家们已经证明，理论上很快破解 RSA 加密](https://blogs.ams.org/mathgradblog/2014/04/30/shors-algorithm-breaking-rsa-encryption/)是可能的。

一旦量子计算机上运行的新算法破解了加密，自然会出现这样的问题:如何保证数据传输的安全？只需看看量子密钥分发(QKD)。这是一种将信息编码成基本粒子的方式，比如光子。让我们看看 QKD 现有的核心组件:

1.  *纠缠。这是一种理论，认为两个粒子可以跨越很远的距离连接起来。这两个粒子永远不会共享同一个状态。如果一个粒子被移动到“向上状态”,第二个粒子将立即变为“向下状态”。这一事实可以被利用来进行交流。*
2.  *叠加。*这个理论说的是一个基本粒子，和光子一样，存在于所有可能的状态，但随后被观测时坍缩为时空中的一点。为了安全起见，可以利用这一点。

在我们的例子中，假设你和一个朋友正在寻求安全的通信。你可以通过纠缠光子来回发送信息，在纠缠光子中你已经对信息进行了编码。

但是黑客对你说的话感兴趣。所以他们拦截了你的信息。然而，叠加原理告诉我们，当一个光子被观测到时，它会发生变化并坍缩。因此，当这种情况意外发生时，您会收到有人试图阅读您的邮件的警报。这种类型的安全系统使用量子力学法则来保护信息。使用物理的巧妙之处在于，你需要打破物理定律来破解信息的密码。这种安全级别的承诺是主要国家投资资源创建这样一个系统的原因。

# 中国的方法:

中国的方式有一种美。他们知道量子计算很重要，他们正在努力推进这项技术。但是量子计算是一种开放的技术。一旦研究科学家们找到了冷却粒子和保持量子比特纠缠的方法，这项技术将会传播开来，中国将会在量子计算领域成为世界第一或第二。这项技术甚至可能商业化。

但是一旦这些量子设备可用，安全性将变得至关重要。这就是为什么中国优先考虑赢得量子密码术这一闭门技术的战斗。第一个发明质数因式分解算法的国家获胜。他们将解密所有民族国家的数据。一旦发生这种情况，它对任何人都没用了。从那里，中国可以利用他们的量子密钥分发安全使*的*通信无法破解。

![](img/61883af876f18bdb445aad22b006d006.png)

China has prioritized Quantum Cryptography over Quantum Computing

# 未来展望:

关于管理量子计算、密码术和算法设计的辩论政策的一个问题是，它是一个抽象的概念，感觉它只会影响后代。因此，辩论往往局限于科学素养的边缘。

然而,《连线》杂志的阿米特·卡特瓦拉描述了科技行业目前的时间表:

> 量子计算机开始走出研究实验室，寻求更广泛的投资和应用。10 月，[谷歌宣布](https://www.technologyreview.com/s/609035/google-reveals-blueprint-for-quantum-supremacy/)到今年年底，它有望实现量子优势——量子计算机可以超越经典计算机。
> 
> 由于包括中国在内的世界各国都在大力投资研发，世界可能在不到十年的时间里就会有一天，一个民族国家可以利用量子计算机让许多当今最复杂的加密系统变得毫无用处。
> 
> 这意味着到今年年底，谷歌将能够比世界历史上任何一台计算机更有效地分解质数。在 10 年内，我们可能会达到一个临界点，所有的通信都被一个政府实体截获和解码。

世界各地的情报机构已经在为这一天做准备，他们可以部署他们的“量子黑客算法”。据报道，这些情报机构正在拦截加密通信，并存储看似无用的字母数字串，等待他们用量子技术解密它们的时机。([来源](https://www.technologyreview.com/s/609035/google-reveals-blueprint-for-quantum-supremacy/))

这将是一场零和游戏。像量子密码这样的闭门技术，只有一个国家/公司能够胜出。唯一的问题是谁？如果让我猜的话，我会把赌注压在政府中 90%的科学家都在做资助决定的国家，而不是超过 1%的国家。