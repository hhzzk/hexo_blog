title: "技术日志:多核"
date: 2015-03-13 17:30:57
tags:
  - 多核
categories:
  - 原创
  - 技术日志
thumbnailImagePosition: left
thumbnailImage: http://7xrt06.com1.z0.glb.clouddn.com/16-3-20/76370097.jpg
---

#### 一、多核并行计算：
<!--more-->
1. 每一个进程都有自己的调用栈（call stack）（用来存储自己的局部变量）、有自己的程序计数器、自己的静态区、堆上new的对象。而每一个线程都有自己的调用栈和程序计数器，并且只能自己访问，但线程直接可以共享静态区和堆上的对象。
2. 进程间通信 （IPC，InterProcess Communication） 除了共享内存（shared memory）之外，还有 管道、消息队列、 信号 、套接口等。

#### 二、java的常量定义用final
例如：
```java
public static final int PI = 3.14；
```
这样在别的类中也可以调用，但是要加上类名。

#### 三、一个程序在不同的线程下花费时间的问题
问题:

    If you have 3 processors available and using 3 threads would take time X , then creating 4 threads would take time 1.5X

总的花费时间为3x， 若划分为4个线程则每个线程执行的时间为3x/4,因为只有3个核，所以必须要执行2个线程的时间才行 所以(3x/4)*2 = 1.5x
***
(EOF)


