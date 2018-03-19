## 区块链行业相关术语中英文对照（持续更新中）

### 为啥要做这个

每一个新兴技术行业的诞生都会随之爆发一堆相关英文术语，不论是技术爱好者还是炒币小韭菜，都会在技术文章或者相关资讯新闻又或者是大咖会议上碰到这类专业名词，第一次听到时估计都是一脸懵逼吧。

[![U5l3R.md.jpg](https://s1.ax2x.com/2018/03/19/U5l3R.md.jpg)](https://simimg.com/i/U5l3R)

尤其某些专家张口闭口专业词汇，听得不少新手云里雾里又不觉明历，我本人也一直深受苦恼，因此决定做一个区块链中英文词汇（目前不仅包含专业术语，也收录常见英文词汇），方便大家理解，本文会不断更新，毕竟币圈一日，人间一年嘛！

***

| 英文      | 中文     | 备注 |
| --------- | -------- | ---- |
| ABI | 智能合约的接口说明 | Application Binary Interface，ABI是以太坊的一种合约间调用的消息格式，类似于WebService的SOAP协议一样，也就是定义操作函数签名，参数编码，返回结果编码等的协议 |
| AES | 高级加密标准 | Advanced Encryption Standard |
| AML       | 反洗钱   | Anti-Money Laundering |
| autonomous | 自治 ||
| ASIC | 专用集成电路技术 | Application Specific Integrated Circuits |
| BIP | 比特币改进建议 | Bitcoin Improvement Proposals |
| BFT | 拜占庭容错 | Byzantine Fault Tolerance |
| Bulletproofs | 防弹证明技术（期待更好的翻译 XD） | 由斯坦福大学提出的，把膨胀系数减少到普通交易的三倍（原来是60倍），可以大幅降低隐私交易的数据量大小的算法 |
| CAP | CAP定理 | 分布式异步网络模型中，不能同时保证一致性，可用性和分区容错性，只能三选二 |
| Corda | | R3联盟推出的金融联盟“类区块链”技术架构，Corda中同样是用交易组成账本，但并没有区块，交易仅在参与方和公证人间传播 |
| consensus | 共识，一致 | |
| Contracts Accounts | 合约账户 | |
| DAO | 分布式自治组织 | Distributed Autonomous Organization，通过一系列公开公正的规则，可以在无人干预和管理的情况下自主运行的组织机构 |
| DAG | 有向无环图 | Directed Acycli Graph，一种共识算法，IOTA项目采用 |
| DBFT | 授权拜占庭容错 | Delegatede BFT，基于权益的BFT共识算法，NEO采用 |
| DD | 尽职调查 | due diligence |
| DLT | 分布式账簿技术 | Distributed Ledger Technology |
| DPOS | 委托权益证明 | Delegated Proof Of Stake，一种共识算法 |
| ECC | 椭圆曲线密码学 | Elliptic Curve Cryptography，一种建立公开密钥加密的算法 |
| EIP | 以太坊改进建议 | Ethereum Improvement Proposals |
| ERC | 以太坊意见征求 | Ethereum Requests for Comment，讨论项目时，一开始会用EIP提出建议，在讨论过程中有一些要征求更多人意见时，就会把细节放在ERC中，而且他们会用同一个号码，比如ERC-20 对应 EIP-20 |
| Ethash | 以太坊挖矿算法 | 以前这个算法称为 Dagger Hashimoto，Ethash是最新版本的 Dagger-Hashimoto改良算法，是Hashimoto算法结合Dagger算法产成的一个新变种。实现两个主要目的：抵抗ASIC矿机和轻客户端易验证 |
| EVM | 以太坊虚拟机 | Ethereum Virtual Machine，借助以太坊虚拟机将solidity代码变成可以在区块链上执行的加密代码。以太坊虚拟机是设计运行在点对点网络中所有参与节点上的一个虚拟机，它可以读写一个区块链中可执行的代码和数据，校验数据签名，并以半图灵完备的方式来运行代码 |
| EOA | 外部账户 | Externally Owned Accounts |
| FLP | FLP定理 | 在网络可靠并且存在节点失效的异步模型中，不存在一个可以解决一致性问题的确定性算法 |
| Frontier | 前沿 | 以太坊开发第一阶段 |
| [geth](https://github.com/ethereum/go-ethereum/wiki/JavaScript-Console) | Go Ethereum | 实现了以太坊协议的javascript运行时环境，可以以交互式或非交互式模式运行 |
| gas | | gas是在以太坊网络中用于衡量执行交易或智能合约工作量的计算单位 |
| gas price | | 以另一种货币或token（例如Ether）计量交易花费的价格。为了稳定消耗gas的价值，gas price是浮动的，根据货币或token价格浮动而相应变动以保持总价格稳定。gas price由市场供需决定（用户愿意支出的价格和矿工节点愿意接受的价格的博弈）|
| gas limit | | 某笔具体的交易能够消耗的gas最大值，一笔标准的以太坊交易需要21，000 gas。当交易的gas limit不足时，会出现 out of gas 错误 |
| [gas used](https://masterthecrypto.com/ethereum-what-is-gas-gas-limit-gas-price/) | | 有效支付用于计算或智能合约运行的gas数量（在成功的交易中gas fee小于 gas limit)，一笔以太坊交易的实际矿工费(Tx Fees) = gas used * gas price |
| HashGraph | 哈希图 | |
| Homestead | 家园 | 以太坊开发第二阶段 |
| [I2P](https://wiki.archlinux.org/index.php/I2P_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)) | | Invisible Internet Project，建立在互联网上的隐匿网络层，用于为网络通讯提供隐私保护 |
| [Infura](https://infura.io/) | | 提供全球范围区块链集群和API端点等基础架构服务；可用于以太坊，IPFS等其他新兴的分布式平台。致力于提供安全，稳定，高容错性金额可扩展的区块链访问接口 |
| IPC | 内部进程间通信 | Inter-Process Communication |
| keccak | | 一种SHA-3加密算法 |
| KYC | 了解你的客户 | Know Your Customer |
| [Kovri](https://getmonero.org/resources/moneropedia/kovri.html) | | I2P网络的C++实现版本，目前还在开发中尚未集成到门罗币中，可以提高交易的安全等级（可以隐藏IP地址） |
| Metropolis | 大都会 | 以太坊开发第三阶段 |
| MD | MD系列消息摘要算法 | 消息摘要算法，包括MD2,MD4,MD5, 安全性依次提升 |
| Oracle | 预言机 | 注意此oracle不是指数据库。预言机连接虚拟与现实，核心功能是提供数据上链服务，是实现智能合约的必要条件。智能合约是在区块链提供的沙盒环境中运行，沙盒是个封闭环境，使合约代码不能读取链外数据，但很多时候智能合约又必须依赖外部数据，oracle在这里就承担了提供外部数据的功能。[参考文章](https://medium.com/taipei-ethereum-meetup/oracle%E7%B3%BB%E5%88%97%E4%B8%80-human-oracle-cb7ed8268030) |
| Paxos | | 一种用于传统分布式系统的共识协议 |
| PBFT | 实用拜占庭容错 | Practical BFT，一种基于BTF的共识算法 |
| Raft | | Paxos协议的一种简单实现 |
| Payment Codes | 可重用支付码 | BIP47，支付码是一种用于创建永久性比特币地址的技术，这些地址可以重复使用，与现实生活中的身份公开相关，同时无损于财务隐私。它们类似于隐形地址。即使他人知道你的支付码也无法追踪你的交易历史，可以用于想要私密的接收BTC的场景 |
| portfolio | 投资组合 ||
| proposal | 提案 ||
| POA | 权威证明 | Proof Of Authority，一种共识算法 |
| POW | 工作量证明 | Proof Of Work，一种共识算法 |
| POS | 权益证明 | Proof Of Stake，一种共识算法 |
| pegged zone | 锚定分区 | 一种锚定分区的桥接机制，出现于Cosmos项目 |
| R&D | 研究与开发 | research and development |
| RPCA | 瑞波共识算法 | Ripple Protocol Consensus Algorithm，类似PBFT的共识机制 |
| [RingCT](https://getmonero.org/resources/moneropedia/ringCT.html) | 环加密交易 | Ring Confidential Transactions，隐藏交易信息（包括交易双方信息和交易金额）的加密技术，门罗币采用 |
| ring signatures | 环签名 | 用于隐匿发送发信息的技术，门罗币采用 |
| [RLP](https://github.com/ethereum/wiki/wiki/%5B%E4%B8%AD%E6%96%87%5D-RLP) | RLP编码 | Recursive Length Prefix（递归长度前缀）是一种适用于任意二进制数据数组的编码。是以太坊中对象进行序列化/反序列化的主要编码方式。区块，交易等数据结构在持久化时会先经过RLP编码后再存储到持久层中 |
| Serenity | 宁静 | 以太坊开发第四阶段（也是最后一个阶段） |
| SHA | SHA系列安全散列算法 | 生成固定长度摘要信息(长度等于版本名中的数字，例如SHA-160的摘要长度就是160位)，包括SHA-1(160)，SHA-2(224，256，384，512) |
| signature | 签名 ||
| stealth address | 隐匿地址 | 能够隐藏接收方信息 |
| swarm | | 去中心化的数据存储访问协议，以ETH作为激励。类似使用了Filecoin的IPFS |
| [testrpc](https://github.com/trufflesuite/ganache-cli) | 以太坊节点客户端 |  是一个基于NodeJS的用于测试开发的以太坊节点客户端。使用ethereumjs模拟客户端行为，包含主要的RPC函数和功能。目前已被收录到truffle的开发工具包中 |
| TX | 交易 | transaction |
| TX id | 交易ID | 即 交易的hash值 |
| Truffle | | 一个基于以太坊技术的开发、测试和部署框架，旨在帮助以太坊开发者更容易开发去中心化应用（DApp） |
| whisper | | 去中心化的通信协议 |
| [YC](http://www.ycombinator.com/) | Y Combinator | 成立于2005年是美国著名创业孵化器，扶持初创企业并为其提供创业指南（Airbnb，Dropbox，Stripe，Reddit, Docker, Coinbase 等）,投资孵化过多个区块链项目 |
| ZKRP | 零知识范围证明 | Zero Knownledge Range Proof，证明一个具体声明的真实性而不会泄露它试图证明的额外信息 |
| ZK-SNARKs | 零知识证明 | ZK-Succint Non-interactive Arguments of Knownledge |
| [2-Way Peg](https://faq.rsk.co/hrf_faq/what-is-the-2-way-peg/) | 双向锚定 | 一种跨链技术 |
