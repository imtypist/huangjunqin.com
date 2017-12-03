---
title: 转载 | 经纬度转平面坐标的算法（米勒投影）
date: 2017-12-04 00:14:03
---

[原文出处：经纬度转平面坐标的算法（米勒投影）](http://xbingoz.com/2012/12/31/Miller-cylindrical/)

地图组件是前端数据可视化非常重要的一个组成部分，根据geoJSON这种通用数据格式来生成地图是比较便捷的做法。不过对于地图坐标转换的算法，还是了解一些比较好，对于设定高阶地图组件会有帮助。这里介绍一下在米勒投影的地图上，如何将经纬度转换为平面坐标的算法，这个算法在生成世界地图的时候比较常见。（维基百科-米勒投影）

```javascript
// lon 经度，西经为负数
// lat 纬度，南纬是负数
function millerXY (lon, lat){
     var L = 6381372 * Math.PI * 2,     // 地球周长
         W = L,     // 平面展开后，x轴等于周长
         H = L / 2,     // y轴约等于周长一半
         mill = 2.3,      // 米勒投影中的一个常数，范围大约在正负2.3之间
         x = lon * Math.PI / 180,     // 将经度从度数转换为弧度
         y = lat * Math.PI / 180;     // 将纬度从度数转换为弧度
     // 这里是米勒投影的转换
     y = 1.25 * Math.log( Math.tan( 0.25 * Math.PI + 0.4 * y ) );
     // 这里将弧度转为实际距离
     x = ( W / 2 ) + ( W / (2 * Math.PI) ) * x;
     y = ( H / 2 ) - ( H / ( 2 * mill ) ) * y;
     // 转换结果的单位是公里
     // 可以根据此结果，算出在某个尺寸的画布上，各个点的坐标
     return {
          x : x,
          y : y
     };
}
```
<!-- more -->
我采用这篇文章的转换算法，对从高德地图获取的经纬度坐标进行转换，然后用转换后平面坐标数据生成sumo路网文件，发现可行，效果如下。

![chengdu](http://cdn.huangjunqin.com/sensor.png)

因为我用的是python处理数据，顺便附上python版实现

```python
import math
def millerXY(lon, lat):
	L = 6381372 * math.pi * 2
	W = L
	H = L / 2
	mill = 2.3
	x = lon * math.pi / 180
	y = lat * math.pi / 180
	y = 1.25 * math.log(math.tan( 0.25 * math.pi + 0.4 * y ))
	x = ( W / 2 ) + ( W / (2 * math.pi) ) * x
	y = ( H / 2 ) - ( H / ( 2 * mill ) ) * y
	return (x,y)
```
