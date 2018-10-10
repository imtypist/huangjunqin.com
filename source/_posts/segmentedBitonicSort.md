---
title: 分段双调排序算法
date: 2017/8/19 16:31:00
---

双调排序是data-independent的排序， 即比较顺序与数据无关的排序方法， 特别适合做并行计算，例如用GPU、fpga来计算。

**1、双调序列**

双调序列是一个先单调递增后单调递减（或者先单调递减后单调递增）的序列。

**2、Batcher定理**

将任意一个长为2n的双调序列A分为等长的两半X和Y，将X中的元素与Y中的元素一一按原序比较，即a[i]与a[i+n] (i < n)比较，将较大者放入MAX序列，较小者放入MIN序列。则得到的MAX和MIN序列仍然是双调序列，并且MAX序列中的任意一个元素不小于MIN序列中的任意一个元素。

```c++
for (i=0;i<n;i++) {
    if (get(i)>get(i+n)) exchange(i,i+n);
}
```

**3、双调排序**

假设我们有一个双调序列，则我们根据Batcher定理，将该序列划分成2个双调序列，然后继续对每个双调序列递归划分，得到更短的双调序列，直到得到的子序列长度为1为止。这时的输出序列按单调递增顺序排列。类似于归并排序。

见下图，升序排序，具体方法是，把一个序列(1…n)对半分，假设n=2^k，然后1和n/2+1比较，小的放上，接下来2和n/2+2比较，小的放上，以此类推；然后看成两个(n/2)长度的序列，因为他们都是双调序列，所以可以重复上面的过程；总共重复k轮，即最后一轮已经是长度是2的序列比较了，就可得到最终的排序结果。

![](http://cdn.huangjunqin.com/1.png)

<!-- more -->

**4、任意序列生成双调序列**

这个过程叫Bitonic merge, 实际上也是divide and conquer的思路。 和前面sort的思路正相反， 是一个bottom up的过程——将两个相邻的，单调性相反的单调序列看作一个双调序列， 每次将这两个相邻的，单调性相反的单调序列merge生成一个新的双调序列， 然后排序（同第3点的双调排序）。 这样只要每次两个相邻长度为n的序列的单调性相反， 就可以通过连接得到一个长度为2n的双调序列，然后对这个2n的序列进行一次双调排序变成有序，然后在把两个相邻的2n序列合并（在排序的时候第一个升序，第二个降序）。 n开始为1， 每次翻倍，直到等于数组长度， 最后就只需要再一遍单方向（单调性）排序了。

以16个元素的array为例：

1. 相邻两个元素合并形成8个单调性相反的单调序列
2. 两两序列合并，形成4个双调序列，分别按相反单调性排序
3. 4个长度为4的相反单调性单调序列，相邻两个合并，生成两个长度为8的双调序列，分别排序
4. 2个长度为8的相反单调性单调序列，相邻两个合并，生成1个长度为16的双调序列，排序

![](http://cdn.huangjunqin.com/2.png)

**5、非2的幂次长度序列排序**

这样的双调排序算法只能应付长度为2的幂的数组。那如何转化为能针对任意长度的数组呢？一个直观的方法就是使用padding。即使用一个定义的最大或者最小者来填充数组，让数组的大小填充到2的幂长度，再进行排序。最后过滤掉那些最大（最小）值即可。这种方式会使用到额外的空间，而且有时候padding的空间比较大（如数组长度为1025个元素，则需要填充到2048个，浪费了大量空间）。但是这种方法比较容易转化为针对GPU的并行算法。所以一般来说，并行计算中常使用双调排序来对一些较小的数组进行排序。 如果要考虑不用padding，用更复杂的处理方法，参考[n!=2^k的双调排序网络](http://blog.csdn.net/ljiabin/article/details/8630627)。

**6、Bitonic Sort** *双调排序参考代码[来源](http://www.tools-of-computing.com/tc/CS/Sorts/bitonic_sort.htm)*

- version Ⅰ（递归）

使用sortup, sortdown, mergeup, mergedown四个函数来排序出不递减(non-decreasing)序列或者不递增(non-increasing)序列，最终递归合并成有序序列。

`void sortup(int m, int n)`将[m,m+n)区间内的n个元素按不递减顺序排列，然后使用`void mergeup(int m, int n)`将该区间的n个元素归并到整个不递减序列中。其余两个函数功能类似。

所以在`main`函数中调用可以是`sortup(0, N)`，N是元素个数。

```c++
void sortup(int m, int n) {//from m to m+n
    if (n==1) return;
    sortup(m,n/2);
    sortdown(m+n/2,n/2);
    mergeup(m,n/2);
}
void sortdown(int m, int n) {//from m to m+n
    if (n==1) return;
    sortup(m,n/2);
    sortdown(m+n/2,n/2);
    mergedown(m,n/2);
}
void mergeup(int m, int n) {
    if (n==0) return;
    int i;
    for (i=0;i<n;i++) {
        if (get(m+i)>get(m+i+n)) exchange(m+i,m+i+n);
    }
    mergeup(m,n/2);
    mergeup(m+n,n/2);
}
void mergedown(int m, int n) {
    if (n==0) return;
    int i;
    for (i=0;i<n;i++) {
        if (get(m+i)<get(m+i+n)) exchange(m+i,m+i+n);
    }
    mergedown(m,n/2);
    mergedown(m+n,n/2);
}
```

- version Ⅱ（并行，非递归）

```c++
int i,j,k;
for (k=2;k<=N;k=2*k) {
  for (j=k>>1;j>0;j=j>>1) {
    for (i=0;i<N;i++) {
      int ixj=i^j;
      if ((ixj)>i) {
        if ((i&k)==0 && get(i)>get(ixj)) exchange(i,ixj);
        if ((i&k)!=0 && get(i)<get(ixj)) exchange(i,ixj);
      }
    }
  }
}
```

**7、分段双调排序实现**

[segmentedBitonicSort源码](https://github.com/imtypist/segmentedBitonicSort)

**8、参考文献**

[[1] 三十分钟理解：双调排序Bitonic Sort，适合并行计算的排序算法](http://www.cnblogs.com/tuding/p/7335853.html)

[[2] Bitonic Sort](http://www.tools-of-computing.com/tc/CS/Sorts/bitonic_sort.htm)

[[3] 分段双调排序实现](http://blog.csdn.net/u014226072/article/details/56840243)
