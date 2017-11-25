----
title: Introduction of Congestion Control Algorithms
date: 2017-07-21 00:08:00
----

今天看了一篇关于虚拟网络拥塞控制的paper，核心思想就是把新的拥塞控制算法通过虚拟机管理程序将其翻译为旧的描述方式，以便于那些传统的、不能更新的应用也可以使用新的算法，[Virtualized Congestion Control(SIGCOMM2016)](http://dl.acm.org/authorize?N19276)。里面提到了三种网络拥塞控制算法：TCP with ECN，DCTCP，TIMELY。在这里分享几篇博文学习一下。
其中涉及到一个**TCP incast**概念：TCP incast是指在数据中心网络中，随着同时参与数据传输的服务器数据增加，产生的数据流量会将瓶颈交换机的缓冲区溢出，引起丢包事件以及随后的数据重传，最终导致系统吞吐量急剧下降。

### TCP with ECN
- [TCP中ECN详细工作机制讲解一](http://blog.csdn.net/feeltouch/article/details/9319585)
- [TCP中ECN的工作原理分析二（摘自：RFC3168）](http://blog.csdn.net/feeltouch/article/details/9319641)

### DCTCP
- [Data Center TCP(DCTCP)](http://blog.chinaunix.net/uid-1728743-id-4945682.html) 

<!-- more -->

### TIMELY
这个算法是来自google2015年在SIGCOMM会议上发表的一篇文章，附上paper地址[TIMELY: RTT-based Congestion Control for the Datacenter(SIGCOMM2015)](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/09/timely.pdf)。











