title: "技术日志: c++中const修饰符作用总结"
date: 2015-03-15 12:30:05
tags:
  - C/C++
  - const
categories:
  - 原创
  - 技术日志
thumbnailImagePosition: left
thumbnailImage: http://7xrt06.com1.z0.glb.clouddn.com/16-3-20/76370097.jpg
---

C++中 `const` 修饰符的使用相当灵活，所以使用起来也比较混乱，不能准确的把握其修饰的变量。
<!--more-->
#### 1.简单的用法：
`const` 最简单的用法就是声明一个常量，这个用法在c、c++中都适用。
例如下面的语句：
```C++
const int constant1 = 100；
```
定义了一个常量 `constant1` ，这样在之后的程序中 `constant1` 的值就不能发生改变，只能是100，否则就会报错。

在c/c++中 也可以用 `#define`  来定义一个常量，但两者有很多差别，例如， `#define` 定义一个宏，没有类型的区别，在预处理时进行替换，而 `const` 是有类型的常量，在程序运行时使用，并且会进行类型检查。`const` 比 `#define` 有更多的优点，所以推荐使用 `const` .

#### 2.修饰指针
`const` 同样可以修饰一个指针，这时就会产生歧义，到底 `const` 修饰的是指针本身还是指针指向的内容。
例如：
```C++
const int *Constant2；
int const * Constant2;
```
>declares that  Constant2  is a variable pointer to a constant integer

上边两个`Const` 的作用是一样的, `const` 修饰的 `constant2` 是一个指针，它指向一个整形常量。也就是说 `const` 修饰的是指针指向的内容。

下面这种声明方式则不同了：
```C++
int * const Constant3;
```
>(declares that  Constant3  is constant pointer to a variable integer)

`const` 修饰的 `Constant3` 是一个常量指针，它指向一个整形变量。也就是说 `const` 修饰的是指针本身。

如果想要既修饰指针本身又修饰指针所指的内容则如下面：
```C++
int const * const Constant4;
```

简单的区分方法是看const左边的关键字，如果左边没有则看右边的关键字，如果关键字是*则修饰的是指针本身，否则反之。
>（对应得在《effctive c++》中的解释，如果关键字const出现在\*号的左边，表示被指物是常量；如果出现在\*号的右边，表示指针自身是常量；如果出现在\*号的两边，表示被指物和指针都是常量）

***
(EOF)
