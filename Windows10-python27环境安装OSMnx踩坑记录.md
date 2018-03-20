---
title: Windows10+python27环境安装OSMnx踩坑记录
date: 2018-03-20 22:03:08
tags:
---
在Win10下安装OSMnx库异常艰难，虽然官方推荐的是用python3以及conda包管理器，但是我还是想用python2+pip来安装，也不是不行，就是坑有点多，摸索之后总算总结了一个经验出来。

我的环境是python27，先提一句，**最好用64位的python**，因为到后面运行数据的时候，因为我的数据量比较大，其实也不大，反正用32位的python运行，一旦运存超过2GB就会报`Memory Error`错误，很是蛋疼。

首先，先去下载这个安装一下，安装OSMnx需要VC 9.0的环境，[Download Microsoft Visual C++ Compiler for Python 2.7 from Official Microsoft Download Center](https://www.microsoft.com/en-us/download/confirmation.aspx?id=44266)

然后，去[Unofficial Windows Binaries for Python Extension Packages](https://www.lfd.uci.edu/~gohlke/pythonlibs/)下载几个whl包，直接安装，因为这几个用pip装不上，会报错...安装包名字为`Rtree`，`Fiona`，`Shapely`，`GDAL`。

```bash
pip install ./path/to/*.whl
```

直接装就行了。其实Rtree用pip直接在线安装不会报错，但是后面你运行的时候就会发现报错说缺少一个DLL，所以还是手动下载，就没问题了。

然后就安装OSMnx

```bash
pip install osmnx
```

就可以顺利安装上OSMnx，因为官方的例子都是在jupyter notebook上运行，所以我们也装一下。

<!-- more -->

```bash
pip install jupyter
```

不出意外就顺利装好了，最后一个点，就是你发现jupyter notebook运行不起来，因为我们用的是python27，所以还得装一个`ipython`，python34以上的版本就可以不用这一步。

```bash
pip install ipython==5.5.0
```

注意版本需要是5.x的，5.5.0是5.x中最新的一个，需要指定版本。到此就可以顺利运行jupyter了，

```bash
jupyter notebook
```

OK，踩坑完毕！

![运行效果图](http://cdn.huangjunqin.com/%E6%8D%95%E8%8E%B7.PNG)
