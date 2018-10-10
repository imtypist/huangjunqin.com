---
title: 转载 | 物联网与『高效的』IOTA
date: 2018-10-10 21:26:29
tags:
---

推荐一篇好文章，[>>原文传送门](https://draveness.me/iota-tangle)

虽然IOTA目前还是有不少缺点，不过其DAG的新颖架构，以及没有交易费，为高频的微交易创造了条件。因为IoT设备所属方本身就比较明确，所以并无需引入外来miner，由所属组织自行控制，所以就不用特别考虑incentive mechanism的设计，所以没有交易费并不算是缺点。

文章里提到密码学中的一条黄金法则

> The golden rule of cryptographic systems is “don’t roll your own crypto.”

永远不要自己发明加密算法！IOTA自己推出的curl哈希算法被爆出可能产生哈希碰撞的问题，所以为了安全起见，他们又将算法切换回SHA3。

另外，看新闻说IOTA下一步/即将支持智能合约，希望快点release :)