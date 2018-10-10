---
title: 【IEEE HotICN 2018】记录论坛及会议的key points
date: 2018-08-16 00:56:33
tags:
---

**keyword**: NDN, blockchain, ICN

**演讲主题列表**：[Program](https://2018.ndnlab.com/index.php?m=content&c=index&a=lists&catid=23)

### 区块链技术与应用论坛（2018.8.14）

- 区块链系统运维与落地应用（严挺）

> 带宽提升，单点计算能力提升，分布式技术的应用
> tamper-proof 防篡改，应用于审计、管控
> 数据半共享、带条件的共享

他强调一个区块链应用的理念，就是技术+业务，更多业务上的了解，对客户以及应用落地比较友好。更多做的也是金融方面的应用，应该是基于Hyperledger Fabric区块链开发的。

- 区块链打破互联网垄断（田鸿飞）

> 传统互联网的垄断商业模式（FANG，BAT）
> 数据孤岛（i.e., lack of sharing）和隐私侵犯，集中式存储的弊端
> 同态加密技术，在加密数据上进行统计数理计算，但是性能可能是问题
> 用户数据与互联网公司独立开，公链

key idea 数字资产化（区块链应用场景），资产数字化

- YeeCall，区块链时代的基础设施（江周平）

> 节点状态同步，共识  分布式网络、丢包
> 自组网（<u>区块链+车联网</u>） PBFT
> zkSNARKs 隐私保护

YeeChain（github开源），新共识机制（Tetris俄罗斯方块）

- Hyperledger Fabric数据隐私保护（赵振华）

> mutichannel 多条链

节点权限控制，是否可访问数据，校验哈希值

- 从信息系统看区块链的未来演进（斯雪明）

> Is blockchain technology practical for data process platform? 利弊
> 网络层 得：分布式 失：监管难度大
> 共识层 得：安全性 失：效率低
> 应用层 DApp 用户发布的智能合约存在漏洞
> 高度同构 高度冗余 高度关联 （往异构、不那么冗余、不那么关联发展）
> 避免智能合约的图灵完备，作何理解？大部分智能合约存在漏洞

性能和安全存在矛盾，PoW消耗资源大，安全；PoS，DPoS性能高，安全性比较低，根据不同的安全、性能需求选择不同的模型。

<!-- more -->

- 一些产业界/工业界应用

![manuscript](http://cdn.huangjunqin.com/IMG_20180816_004951.jpg)

### 未来网络技术与工程国际大会（2018.8.15）

- 构建泛在云化网络（刘韵洁）

> CDN（Content Delivery Network）
> SDN（Software Defined Network）
> NDN（Named Data Networking）

IPv4地址分配枯竭，提出未来网络；浮空平台通信网络，（飞艇/其他飞行物）在平流层停留比较长的一段时间；海陆空一体化网络

- Think Sequential, Run Parallel（樊文飞）

> Graph Computation，模式匹配
> 并行图计算

- 新技术融合下的区块链应用趋势（陈晓红）

> 物联网问题：设备安全，个人隐私，架构僵化，通信兼容，多主体
> 区块链TPS是主要问题，数据分片

区块链系统缺乏标准化；从技术底层上改进方向：组网方式、共识算法，交易安全等等。

- Modernizing Cyberspace from the Network Ground Up（张丽霞）

> IPv4地址资源枯竭 ---> NDN，解决问题的同时继承了IP地址的优点 end2end principle

- NDN Tutorial（张北川）

![manuscript](http://cdn.huangjunqin.com/IMG_20180816_005301.jpg)

- UDN超密集网络 in 5G（张海军）

![manuscript](http://cdn.huangjunqin.com/IMG_20180816_005325.jpg)
