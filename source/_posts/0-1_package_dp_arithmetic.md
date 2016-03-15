title: "0-1背包问题的动态规划算法"
date: 2014-12-13 21:50:54
tags:
  - 0-1背包
  - 算法
  - 动态规划
categories:
  - 原创
  - 算法系列
thumbnailImagePosition: left
thumbnailImage: http://7xrt06.com1.z0.glb.clouddn.com/16-3-15/63205077.jpg
---

0-1背包问题是最基本的背包问题，本篇介绍了其动态规划的解法。
<!-- excerpt -->

#### 【问题描述】
&emsp;&emsp;有一个贼在偷窃一家商店时发现n件物品，其中第i件物品的价值为Vi 元，重wi千克。此处的Vi 和wi都为整数。他希望带走的东西越值钱越好，但他的背包只能放下W千克的物品，W为一整数。那么他应该带走那几件物品呢？

&emsp;在上面的问题中所有物品只能被留下或者带走，不存在带走一部分的情况，所以问题被称为0-1背包问题。

#### 【算法原理】
&emsp;&emsp;首先了解一下几个表达式的含义：

- `v[1...n]` : 表示n个物品的价值；
- `w[1...n]` : 表示n个物品的重量；
- `maxVal[weight][num]`:其中保存前num件物品，承载量为weight千克的背包所能得到的最大价值；
- `maxVal(weight, num)` : 表示对于前num件物品，承载量为weight千克的背包所能得到的最大价值。

&emsp;&emsp;面对所有的物品，小偷要考虑的就是拿或者不拿，哪一种方案得到的物品价值最大。假设从物品n开始考虑，如果拿走第n件物品，则小偷得到了第n件物品的价值vn，相应的小偷的背包只能放下 `W-wi`  千克重的物品了，表达式 `maxVal(W-wi, n-1)` 表示前 `n-1` 个物品，承载量为 `W-wi` 千克的背包得到的最大价值。所以如果小偷拿走第n件物品所能得到的最大价值为： `vn + maxVal(W-wi, n-1) ` 。如果不拿走第n件物品，那么小偷仍然有承载量为W千克的背包，所以小偷能得到的最大价值就是W千克的背包在前 `n-1` 个物品中所能获得的最大价值: `maxVal(W, n-1)` 。小偷知道了在两种不同的情况下所能获得最大价值后就可以对第n件物品进行取舍了，所以能获得的最大价值为：

    maxVal(W, n) = max {vn + maxVal(W-wi, n-1)， maxVal(W, n-1)}

&emsp;&emsp;很显然，上式是一个递归式，并且当wight=0或者num=0时， `maxVal(weight, num)` 的值为0，当 `weight < wnun` ，即背包所能承载的重量放不下当前的物品时， `maxVal(weight, num)` 的值为 `maxVal(weight, num-1)` ，因为第num件物品不存在放不放的问题了，根本就放不进去。所以有：

![gongshi](http://ww4.sinaimg.cn/large/9d15fcebgw1erosbbs2nwj20ks02rdg0.jpg)

&emsp;&emsp;由此我们可以很轻松写出动态规划的算法，注意在递归函数中首先要判断 `maxVal[W][n]` 中是否已经存储了要获取的值，避免重复计算，这也是动态规划算法的优势所在。

&emsp;&emsp;根据保存在数组 `maxVal[W][n]` 中的值，我们可以利用回溯法得到最优子集。基本思想是如果拿掉第n件物品，获取的最大价值 `maxVal[W][n]` 等于 `maxVal[W-wn][n-1] + vn` ，则第n件物品拿走，否则相反。然后依次类推。

#### 【代码实现】
```C
#include<stdio.h>
#include"common.h"

// Record goods max value 
static int gMaxValue[PACKAGE_CAPACITY+1][GOODS_MAX_KIND+1];

int maxValue(Goods *goods[], int bagCap, int goodsNum)
{
    int value1 = 0;
    int value2 = 0;
    int maxVal = 0;
    int weight = 0;
    
    if(gMaxValue[bagCap][goodsNum] != 0)
        return gMaxValue[bagCap][goodsNum];

    if(goodsNum <= 0)
        return 0;
    // Only one goods
    if(goodsNum == 1)
    {
        if(bagCap >= goods[goodsNum-1] -> Weight)
            maxVal = goods[goodsNum-1] -> Value;
    }
    else
    {
        weight = goods[goodsNum-1]->Weight;

        // Goods can not put in to the bag 
        if(bagCap < weight)
        {
            maxVal = maxValue(goods, bagCap, goodsNum-1);
        }
        // Judge the max value
        else
        {
            value1 = goods[goodsNum-1]->Value + maxValue(goods, bagCap-weight, goodsNum-1);
            value2 = maxValue(goods, bagCap, goodsNum-1);
        
            maxVal = value1 > value2 ? value1 : value2;
        }
    }
    gMaxValue[bagCap][goodsNum] = maxVal;

    return maxVal;
}

// Get the selected goods
int getGoods(Goods *goods[], int flag[], int goodsNum)
{
    int i = PACKAGE_CAPACITY;
    int j = goodsNum;

    while(gMaxValue[i][j] != 0)
    {
        if(gMaxValue[i][j]-goods[j-1]->Value == gMaxValue[i-(int)goods[j-1]->Weight][j-1])
        {
            flag[j] = 1;
            i = i-goods[j-1]->Weight;
            j = j - 1;
        }
        else
        {
            j = j-1;
        }
    }

    return 0;
}

int main()
{
    int i = 0;
    int j = 0;
    int realNum = 0;
    int maxVal = 0;
    float W = PACKAGE_CAPACITY;
    int flag[GOODS_MAX_KIND+1] = {0};
    Goods *goods[GOODS_MAX_KIND] = {NULL};

    for(i=0; i <= W; i++ )
    {
        for(j=0; j<=GOODS_MAX_KIND; j++)
            gMaxValue[i][j] = 0;
    }

    realNum = getGoodsInformation(goods);

   // sortByPrice(goods, realNum);

    maxVal = maxValue(goods, W, realNum);
    
    getGoods(goods, flag, realNum);
    
    for(i=0; i<GOODS_MAX_KIND+1;i++)
    {
        if(flag[i] == 1)
            printf("%d ,", i);
    }
    printf("\n");

    printf("The total is %d\n",maxVal);

    return 0;
}
```

#### 【参考资料】

- 一篇很棒的解释动态规划的文章：[通过金矿模型介绍动态规划](http://www.cnblogs.com/SDJL/archive/2008/08/22/1274312.html)
- [回溯法图解](http://www.cnblogs.com/lpshou/archive/2012/04/17/2454009.html)

***
(EOF)
