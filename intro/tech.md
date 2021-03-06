## 关键技术和挑战

从技术角度讲，区块链涉及到的领域比较杂，包括分布式、存储、密码学、心理学、经济学、博弈论、网络协议等，下面列出了目前认为有待解决或改进的关键技术点。

### 密码学认证技术

怎么防止交易记录被篡改？

怎么证明交易方的身份？

怎么保护交易双方的隐私？

密码学正是解决这些关键问题的有效手段。包括 hash 算法，加解密算法，数字证书和签名等。

区块链技术的应用将可能刺激密码学的进一步发展，包括加密强度、加解密处理的性能等。但这将依赖于数学科学的进一步发展和新一代计算技术的突破。

### 分布式一致性

这是个古老的话题，已有大量的研究成果。

核心在于解决如何确保写入的信息是合法的，是被大家都承认的，同时这个信息是被确定的，不可推翻的。

比特币区块链引入了“工作量证明”（Proof of Work）策略来规避少数人恶意破坏数据，并通过概率模型保证最后大家看到的就是合法的最长链。此外，还有以权益为抵押的 PoS、DPoS 和 Casper 等。这些算法的思想上都是基于经济利益的博弈，让恶意破坏的参与者损失经济利益，从而保证大部分人的合作。同时，确认必须经过多个区块之后从概率学上进行保证。

更广泛的区块链技术引入了更多的一致性技术，包括经典的拜占庭算法等，可以解决确定性的问题。

一致性问题在很长一段时间内都将是最具学术价值的研究热点，一个核心的指标将是容错的节点比例。PoW 等系列算法理论上允许少于一半的不合作节点，PBFT 等算法理论上允许不超过 $$\frac{1}{3}$$ 的不合作节点。

### 性能

如何提高交易的吞吐量，同时降低交易的确认延迟。

目前，公开的比特币区块链只能支持平均每秒约 7 笔的吞吐量，一般认为对于大额交易来说，安全的交易确认时间为一个小时。小额交易只要确认被广播到网络中并带有交易服务费用，即有较大概率被最终打包到区块中。

区块链系统跟传统分布式系统不同，其处理性能无法通过单纯增加节点数来进行扩展，实际上，很大程度上取决于单个节点的处理能力。**高性能、安全、稳定性、硬件的加解密能力**，都将是考察节点的核心方面。

一方面可以将单个节点采用高性能的处理硬件，同时设计优化的策略和算法，提高性能；另外一方面将大量高频的交易放到链外来，只用区块链记录最终交易信息，如 [闪电网络]() 等。类似的，侧链（side chain）、影子链（shadow chain）等的思路在当前也有一定的借鉴意义。这样的设计可以很容易的将交易性能提升几个数量级。此外，如果采用联盟链的方式，降低对信任性的要求门槛，也可以换来性能的提升。

目前，开源区块链自身在平台层面已经实现单客户端每秒数百次的交易吞吐量（参考后面的 [性能评测数据](https://github.com/yeasy/blockchain_guide/blob/master/hyperledger)），乐观预测将很快突破每秒数千次的基准线，但离证券交易系统的每秒数万笔的峰值还是有较大差距。

实际在工程设计上，性能都存在可以优化的地方。

*注：VISA 系统的处理均值为 2000 tps，号称的峰值为 56,000 tps；证券交易所号称的处理均值在 80,000 tps。*

### 扩展性

普通的分布式系统，可以通过增加节点来提升整个系统的处理能力。

对于区块链网络来说，这个问题并非那么简单。

对于区块链网络来说，每个参与维护的核心节点都要保持一份完整的存储，并且进行智能合约的处理。因此，整个网络的存储和计算能力，目前无法超过单个节点。甚至当网络中节点数过多时，会因为一致性的达成过程延迟降低整个网络的性能。在公有网络中由于大量低质量处理节点的存在问题将更明显。

比较直接的一些思路是放松对每个节点都必须参与完整处理的限制（但至少部分节点要能合作完成一个完整的处理），同时尽量减少核心层的处理工作。同时采用联盟链的模式，采用高性能的节点作为核心节点。

### 安全

既然区块链一个可能的应用前景是金融系统，那么安全自然是讨论最多、挑战最多的话题。区块链的实现是开源的，基于了现有的成熟的密码学算法。但这是否就能确保其安全呢？

**世界上并没有绝对安全的系统。**

而且系统是由人设计的，系统也是由人来运营的，有人参与的系统，更容易出现漏洞。

_可以参考，著名黑客米特尼克所著的《反欺骗的艺术——世界传奇黑客的经历分享》，介绍了大量的实际社交工程欺骗场景。_

有如下几个方面是很难逃避的。

首先是，攻击区块链系统是否是犯罪？攻击银行系统是要承担后果的。但是目前还没有任何法律保护区块链以及基于它的实现。

其次是软件实现的潜在漏洞是无法避免的。考虑到使用了几十年的 openssl 还带着那么低级的漏洞（[heart bleeding](https://heartbleed.com/)），而且是源代码就在大家眼皮底下。这背后曾经发生过啥，让人遐想连篇。对于金融系统来说，包括客户端和平台侧，即便是很小的漏洞都可能造成难以估计的损失。

另外，区块链所有交易记录都是公开可见的。搞大数据的人听了是不是开始激动起来了，确实，这里面能分析的东西还真不少，而且规模够大、影响力够大……实际上，已有文献证明，比特币区块链的交易记录最终是能追踪到用户的。

还有就是作为一套完全的分布式系统，区块链缺乏足够的调整机制，一旦运行起来，真的无人能控制，出现问题也难以修正。即使是让它变得更公平、更完善的修改，只要有部分既得利益者合起来反对，那就无法加入进去。这让比特币本身的价值也蒙上了一层阴影。

此外，运行在区块链上的智能合约应用可能是五花八门的，必须要有办法进行安全管控，在注册和运行前需要有机制进行探测，以规避恶意代码的破坏。

2016 年 6 月 17 日，发生 [DAO 系统漏洞被利用](https://blog.daohub.org/the-dao-is-under-attack-8d18ca45011b) 事件，直接导致价值 6000 万美元的数字货币被利用者获取。尽管对于这件事情的反思还在进行中，但事实再次证明，目前基于区块链技术进行生产应用时，务必要细心谨慎地进行设计和验证。

### 数据库

区块链网络中的块信息需要写到数据库中进行存储。

观察区块链的应用，大量的写操作、hash 计算和验证操作，跟传统数据库的行为十分不同。

当年，人们观察到互联网应用大量非事务性的查询操作，而设计了非关系型（NoSql）数据库。那么，针对区块链应用的这些特点，是否可以设计出一些特殊的针对性的数据库呢？

levelDB、RocksDB 等键值数据库，具备很高的随机写和顺序读\/写性能，以及相对较差随机读的性能，被广泛应用到了区块链信息存储中。但目前来看，区块链数据库仍然是需要突破的技术难点之一。

笔者认为，未来将可能出现更具针对性的“块数据库（BlockDB）”，专门服务类似区块链这样的新型数据业务，其中每条记录将包括一个完整的区块信息，并天然的跟历史信息进行关联。所有操作的最小单位将是区块。

### 其它

区块链的应用也带来了对很多问题的新思考和新需求。

例如：

* 智能合约的合法性和安全性；
* 智能合约代码执行的管控；
* 如何将现实中的合约和条约对应为电子合约；
* 分布式系统的伸缩和迁移；
* 对存储系统新的挑战，特别是性能。

