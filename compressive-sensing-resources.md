----

title: Compressive Sensing Resources | Rice DSP

date: 2017-11-21 20:14:00

----

压缩感知理论指出：只要信号是可压缩的或在某个变换域是稀疏的，那么就可以用一个与变换基不相关的观测矩阵将变换所得高维信号投影到一个低维空间上，然后通过求解一个优化问题就可以从这些少量的投影中以高概率重构出原信号，可以证明这样的投影包含了重构信号的足够信息。

这里分享一个Rice University关于压缩感知的资料集，以下

The dogma of signal processing maintains that a signal must be sampled at a rate at least twice its highest frequency in order to be represented without error. However, in practice, we often compress the data soon after sensing, trading off signal representation complexity (bits) for some error (consider JPEG image compression in digital cameras, for example). Clearly, this is wasteful of valuable sensing resources. Over the past few years, a new theory of "compressive sensing" has begun to emerge, in which the signal is sampled (and simultaneously compressed) at a greatly reduced rate.

As the compressive sensing research community continues to expand rapidly, it behooves us [to heed Shannon's advice](http://arquivo.pt/noFrame/replay/20160516193220/https://dsp.rice.edu/sites/dsp.rice.edu/files/shannon-bandwagon.pdf).

Compressive sensing is also referred to in the literature by the terms: compressed sensing, compressive sampling, and sketching/heavy-hitters.

<!-- more -->

<iframe style="background-color: #fff;height:600px;width: 100%;" src="http://arquivo.pt/noFrame/replay/20160516193220/http://dsp.rice.edu/cs" seamless="seamless" frameborder="0" scrolling="yes"></iframe>
