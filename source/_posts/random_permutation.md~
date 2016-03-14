title: "随机排列"
date: 2015-04-01 11:05:57
tags:
  - 面试题
  - 随机排列
  - 算法
categories:
  - 原创
  - 算法系列
thumbnailImagePosition: left
thumbnailImage: http://7xrt06.com1.z0.glb.clouddn.com/16-3-13/35334370.jpg
---

随机排列的讲解及代码实现
<!-- excerpt -->

**问题描述**
    
>有1到n的n个数，对其进行随机排列组合，每一种排列组合的概率都为 `1/n!` 。要求不能使用额外的空间，可以使用random（int min， int max）函数，此函数表示产生一个min到max之间的随机数。

题目中主要有两点：
1. 从1到n肯定不能有重复；
2. 每一个数出现的概率必须相同。

**做法**
数组元素为array[n] = {1, 2, 3 ... n}。

第一步，随机的产生0到n-1的随机数r0，然后将 `array[r0]` 与第一个元素 `array[0]` 交换；
第二步， 随机的产生1到n-1的随机数r1，然后将 `array[r1]` 与第一个元素 `array[1]` 交换；
...
...
第n-1步，随机的产生n-2到n-1的随机数r(n-2)，然后将 `array[r(n-2)]` 与第一个元素 `array[n-2]` 交换；
第n步，不用交换了

这样产生的随机排列组合的概率一定是1/n!。

**代码如下**

```C
void randomPermutation(array, int count)
{
    for(int i = 0; i < count; ++i) {
        swap(array[i], array[random(i, count)]);  
    }
}
```
***
(EOF)
