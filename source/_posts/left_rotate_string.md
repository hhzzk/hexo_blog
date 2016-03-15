title: "字符串左移、字符串旋转"
date: 2015-04-04 16:54:57
tags:
  - 面试题
  - 算法
  - 字符串旋转
categories:
  - 原创
  - 算法系列
thumbnailImagePosition: left
thumbnailImage: http://7xrt06.com1.z0.glb.clouddn.com/16-3-13/35334370.jpg
---

时间复杂度为O(n),空间复杂度为O(1)的要求下左移字符串。
<!-- excerpt -->

**题目**
>字符串左移:
>void *pszStringRotate( char *pszString, int nCharsRotate)
>比如ABCDEFG，移3位变DEFGABC，要求空间复杂度O（1），时间复杂度O（n）。

题目的要求其实就是把字符串前面的几位移动到字符串的后面，如果题目没有时间复杂度和空间复杂度的要求当然很简单，既可以一位一位的移动，靠牺牲时间来实现，也可以新开辟一个空间，靠牺牲空间来实现。

为了满足时间和空间的要求，可以采用下面这种三步反转的方法：

1. 将字符串分为两部分：ABC 和 DEFG
2. 将这两部分分别反转：CBA 和 GFED
3. 将分别反转后的字符串整个反转：DEFGABC

这样就得到了想要的结果

代码实现（引用自July）
```C
voidReverseString(char* s,int from,int to)
{ while (from < to)
    { char t = s[from];
        s[from++] = s[to];
        s[to--] = t;
    }
} 
voidLeftRotateString(char* s,int n,int m)
{
    //若要左移动大于n位，那么和%n 是等价的
    m %= n;
    //反转[0..m - 1]，套用到上面举的例子中，就是X->X^T，即 abc->cba
    ReverseString(s, 0, m - 1);
    //反转[m..n - 1]，例如Y->Y^T，即 def->fed
    ReverseString(s, m, n - 1);
    //反转[0..n - 1]，即如整个反转，(X^TY^T)^T=YX，即 cbafed->defabc。
    ReverseString(s, 0, n - 1); 
}
```

这个算法既满足空间复杂度O(1),又满足时间复杂度O(n).
***
(EOF)
