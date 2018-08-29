---
title: 如何使用迅雷快速下载Genymotion镜像
date: 2018-04-18 18:32:39
tags:
---

做安卓开发的同学一定知道Genymotion这一著名的安卓模拟器，可惜下载它的ova镜像实在是太太太太慢了！所以，在这里给大家分享一个用迅雷来下载镜像的好方法。

以最新版的`2.12.0`为例，首先，我们可以发现，Genymotion下载的镜像会保存在`C:/Users/你的用户名/AppData/Local/Genymobile/Genymotion/ova`大概是这样子的路径下，你每次选择一个安卓镜像下载的时候都会在这个目录下创建一个临时文件，比如我下载一个安卓5.1.0的镜像，这个目录下的临时文件名字就是`genymotion_vbox86p_5.1_180219_000000.ova.partial`，可以看到是一个部分下载的文件。

这个时候，我们先取消镜像的下载，然后复制上这个临时文件的名字（除了`.partial`）,加上一个前缀，还是以上面这个为例，那么完整的下载地址就是`http://files2.genymotion.com/dists/5.1.0/ova/genymotion_vbox86p_5.1_180219_000000.ova`，复制到迅雷中下载，速度非常快！

大概可以发现规律，下载链接的组成：`http://files2.genymotion.com/dists/<安卓版本号>/ova/目录下临时文件的名字.ova`，下载完成后替换掉ova目录下的临时文件，然后**再去Genymotion选择下载这个镜像**，就会发现已经下载好啦，进入自动部署的阶段了，就可以用啦~

然后关于Genymotion不能安装arm架构的应用，我们可以使用Genymotion-ARM-Translation进行转换，我这边有适用`4.x`，`5.0`，`5.1`的translation工具包，如果有需要的朋友可以通过github的邮件向我要一份~
