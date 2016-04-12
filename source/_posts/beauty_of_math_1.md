title: "编程之美：数学之魅（1）"
date: 2016-04-01 21:05:57
tags:
  - 面试题
  - 算法
categories:
  - 原创
  - 算法系列
thumbnailImagePosition: left
thumbnailImage: http://7xrt06.com1.z0.glb.clouddn.com/16-4-12/14915809.jpg
---

求二进制数中１的个数、发现发帖水王、数组循环移位。
<!-- excerpt -->

## 一、求二进制数中1的个数
对于一个字节（8bit）的无符号整型变量，求其二进制表示中“1”的个数，要求算法的执行效率尽可能高。

**方案一**
常规的乘除法
我们知道当一个数除以2时，如果余数是0，则说明其二进制表示的最后一位是1，相反，如果余数是1，则说明其二进制表示的最后一位是0。 例如：
6（00000110） 除以 2 时，余数为 0 。
3（00000011） 除以 2 时，余数为 1 。
根据这个规律特点，每次除以 2， 判断余数即可以得到 1 的个数。
```c
int Count(BYTE v)
{
    int num = 0;
    while(v)
    {
        if(v % 2 == 1)
        {
            num++;
        }
        v = v / 2;
    }
    return num;
}
```
实际上我们知道，当除以 2 时，也就是右移一位。所以这个问题也可以换算成位移运算。变量 v 每次右移一位并且与0x01（00000001）相与，结果为1则说明最后一位为1.
```c
int Count(BYTE v)
{
    int num = 0;
    while(v)
    {
        num += v & 0x01;
        v >>= 1;
    }
    return num;
}
```
此算法的时间复杂度为Ｏ(logV)

**方案二**
有这样一个规律，变量 v 和　ｖ-1 相与可以去掉其二进制表示中的一个１，　例如：
10(00001010) 和　９（00001001） 相与是　８　(00001000), 可以看出10 的二进制表示变成8之后少了一个　１　。根据这一规律
```c
int Count(BYTE v)
{
    int num = 0;
    while(v)
    {
     v &= (v-1);
     num++;
    }
     return num;
}
```

## 二. 发现发帖 "水王"
在论坛中，某个ID发帖数目超过了帖子总数的一半。如何快速找到这个ID? 问题实际就是找出重复超过一半以上的那个ID.

**方案一**
最直接的方法是扫描这个ID列表，统计每个ID出现的次数，当某个ID次数超过一半是，返回即可。这需要额外的内存空间来保存每个ID出现的次数。

**方案二**
第二种方法是将整个ID列表排序，因为　“水王”　的ID出现的次数超过总数的一半，所以无论如何，排序后列表中间的那个ID一定是水王的ID。但这种方法需要一定的时间复杂度，这取决于采用的排序方法。
第三种方法非常巧妙，每次删除两个不同的ID，那么最后当无法找到不同的ID时，剩下的所有ID一定是水王的。这个方法总的时间复杂度只有O(N),且只需要常数的额外空间。下面的伪代码设计也十分巧妙，只需要遍历一次列表即可：
```c
Type Find(Type* ID, int N)
{
    Type candidate;
    int nTimes = 0;
    int i = 0;
    for(i = 0, nTimes = 0; i < N; i++)
    {
        if(nTimes == 0)
        {
            candidate = ID[i];
            nTImes = 1;
        }
        else
        {
            if(candidate == ID[i])
                nTimes++;
            else
                nTimes--;
        }
    }
    return candidate;
}
```

## 三. 数组循环移位
设计一个算法，把一个含有N个元素的数组循环右移K位，要求时间复杂度为O(N),且只允许使用两个附加变量。
这个题目和[字符串左移、字符串旋转](http://hhzzk.xyz/2015/04/04/left_rotate_string/)是一样的。

***
(EOF)
