---
title: 调频连续波FMCW测距原理
date: 2017-08-06 00:07:00
---

FMCW全拼Frequency Modulated Continuous Wave，这周看的MOBICOM2016关于声信号（Acoustic Signal）跟踪手势的几篇论文中提到的一个比较重要的声波测距技术。

FMCW技术和脉冲雷达技术是两种在高精度雷达测距中使用的技术。其基本原理为，发射波为高频连续波，其频率随时间按照三角波规律变化。雷达接收的回波的频率与发射的频率变化规律相同，都是三角波规律，只是有一个时间差，利用这个微小的时间差可计算出目标距离。

我们来通过MOBICOM2016的一篇论文 [CAT: High-Precision Acoustic Motion Tracking](http://www.cs.utexas.edu/~wmao/resources/papers/cat.pdf) 学习一下FMCW技术的测距原理，下图摘自该篇论文。

![Traditional FMCW](http://cdn.huangjunqin.com/QQ%E6%88%AA%E5%9B%BE20170806002019.png)

<!-- more -->

其中，Transmitted就是扬声器发出的声波，Received就是麦克风接收到的声波信号。

可以看到，在每个周期内信号频率在fmin~fmax之间线性变化。所以在每次扫描的频率可以公式化的表示为`f=fmin+Bt/T`，其中，B表示信号带宽，也就是`fmax-fmin`的值，t是时间，T是一次扫描的时间。这样就可以得到它对应的相位以及cos曲线公式，如下图所示。

![](http://cdn.huangjunqin.com/QQ%E6%88%AA%E5%9B%BE20170806003623.png)

那么对应的，receiver有一个td的延迟时间，同样推算出类似于上图的公式Vr。

然后将`Vt*Vr`得到一个`Vm`，即两个cos函数相乘，可以通过公式`(cos(A-B)+cos(A+B))/2`转换出如下公式：

![](http://cdn.huangjunqin.com/QQ%E6%88%AA%E5%9B%BE20170806004630.png)

假设移动设备距离扬声器距离为R，而且正以v的速度移动，那么td就可以被计算出来`(R+vt')/c`。带入上面的公式。因为相位`phase=(wt'+constant)`，w是角速度，所以w可以由相位phase对`t'`求导数得到，而且频率`f=w/2π`，所以可以得到以下公式。

![](http://cdn.huangjunqin.com/QQ%E6%88%AA%E5%9B%BE20170806010145.png)

当v趋近于0时，得到一个极值`fp=BR/cT`。在这个例子中信号传播路径唯一，`fp`是transmitter和receiver混合信号的频率，由第一次出现的极值就可以确定`fp`。所以根据测量得到的`fp`我们就能够计算出距离R。

即`R=fpcT/B`，从而计算出transmitter和receiver之间的距离。
