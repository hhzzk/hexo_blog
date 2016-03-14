title: "\"Smashing The Stack For Fun And Profit\" 解读"
date: 2014-09-22 00:46:21
tags: 
  - buffer overflow 
  - 栈溢出
  - 黑客
categories:
  - 翻译
thumbnailImagePosition: left
thumbnailImage: http://7xrt06.com1.z0.glb.clouddn.com/16-3-13/34202687.jpg
---

文章翻译解读了 Alpha One 关于堆栈溢出的论文 "Smashing The Stack For Fun And Profit"。
<!-- excerpt -->
<!-- toc -->
&emsp;&emsp;堆栈溢出攻击是黑客攻击中最常用的攻击手段，臭名昭著的Morris worm是世界上第一个用到堆栈溢出攻击的病毒，据统计在1988年破坏了6000台主机。而在8年之后的1996年，Alpha One 通过发表在Phrack杂志上的论文 "Smashing The Stack For Fun And Profit" 让堆栈溢出攻击的原理公开，从此这一攻击手法变成了黑客“入门级别”。

&emsp;&emsp;因为课程安排要求读这个paper，所以逐段做解读：

#### 一.简介：
&emsp;&emsp;在这一部分中讲到了写这篇论文的原因，论文要阐明的事情即什么是缓冲区溢出以及缓冲区溢出攻击是怎么工作的.论文中实验环境是x86和linux系统。

#### 二.进程在内存中的组织方式：
&emsp;&emsp;这一部分是讲进程在内存中的组织形式。进程在内存中区分成三个区域：代码段（Text）、数据段（Data）和堆栈（Stack）。由上图可以看到在内存中的分布是由低地址到高地址。实际上这里他只是做了一个简单的介绍，程序在内存中的分配要复杂的多，下面的图更详细一些（出自《UNIX环境高级编程》）：

![stack](http://ww2.sinaimg.cn/large/9d15fcebgw1erolno1lo8j20cr0amt8w.jpg)

各段中存放的内容如下：

* 代码段：全局常量（const）、字符串常量、函数以及编译时可决定的某些东西
* 数据段（初始化）：初始化的全局变量、初始化的静态变量（全局的和局部的）
* 数据段（未初始化）（BSS）：未初始化的全局变量、未初始化的静态变量（全局的和局部的）
* 堆：动态分配的区域（malloc、new等）
* 栈：用户程序的函数栈帧（包括参数、初始化以及未初始化的局部变量，但不包含静态变量、局部常量（const）），用于调度函数的调用执行
* 命令行参数和环境变量顾名思义存放命令行参数和环境变量

注意：
 1. 对于代码段只有读和执行的权限，对于数据段只有读写的权限。当对他们做了非法的操作时便会出现段错误。
 2. 对于代码段和数据段在程序编译时就已经分配好，而堆和栈是在程序运行时才进行分配的。

##### 2.1 what is a stack & why do we use a stack
&emsp;&emsp;这一部分是讲栈的性质也就是先进后出。但是我们为什么要用到栈，因为现代计算机的设计都是需要高级语言的，而高级语言的主要技术特性就是过程和函数。在程序运行时会调用函数，这就像jump指令一样，但是不同于jump的是，被调用的函数执行完之后，还要返回到之前的语句继续执行，这就用到了栈的先进先出的性质。

##### 2.2 The Stack Region
&emsp;&emsp;栈是包含数据的连续的内存块，寄存器SP指向栈顶，栈底是固定的地址。他的大小在程序运行时自动变化。cpu执行push和pop指令。栈由逻辑堆栈帧组成，当调用函数时他会被push，返回时会被pop。一个堆栈帧由以下几部分组成：

* 函数的返回地址和参数
* 临时变量：包括函数的非静态局部变量以及编译器自动生成的其他临时变量
* 保存的上下文：包括在函数调用前后需要保持不变的寄存器

对于这一部分的内容在《程序员的自我修养》一书中有很详细的讲解，摘录如下：

![stact_region_1](http://ww4.sinaimg.cn/large/9d15fcebgw1eroloedqqjj20e701gdfs.jpg)
![stact_region_2](http://ww4.sinaimg.cn/large/9d15fcebgw1erolpblytjj20ef0cpgmg.jpg)
![stact_region_3](http://ww4.sinaimg.cn/large/9d15fcebgw1erolppojbwj20ea06u0t5.jpg)
![stact_region_4](http://ww1.sinaimg.cn/large/9d15fcebgw1erolpzo3abj20eb05e3yw.jpg)

好了，下面直接上例子：

```C
//example1.c
void function(int a, int b, int c)
{
    char buffer1[5];
    char buffer2[10];
}

void main()
{
    function(1,2,3);
}
```

为了观察函数是怎么被调用的我们加上-S参数，生成汇编代码：

```shell
gcc -S -o example1.s example1.c
```

在 `example1.s` 中我们看到调用 `function` 函数被翻译成(下边是我的执行结果，和论文中有点不一样，但意思是一样的)：

```C
movl $3, 8(%esp)
movl $2, 4(%esp)
movl $1, (%esp)
call function
```

即，先将3个参数入栈然后调用 `function()`，在执行 `call` 指令时，会将下一跳指令 `ret` 压栈。而在 `function()` 函数中的操作如下：

```C
pushl %ebp
movl %esp,%ebp
subl $20,%esp
```

先将老的 `ebp` 压栈，再将现在的 `esp` 扶植到 `ebp` ，然后分配两个数组的空间。

注意，因为有内存对齐的原因这里的空间是增加20个字节而不是15个字节。

所以这个堆栈帧的情况如下：

![stack_function](http://ww3.sinaimg.cn/large/9d15fcebgw1erolqaoeq7j20h703mwef.jpg)

#### 三.缓冲区溢出
&emsp;&emsp;缓冲区溢出是因为向缓冲区中装入了过多的数据，从而导致其溢出。怎么样利用这一常见的程序错误去执行任意的代码呢？看下面的例子：

```C
//example2.c
void function(char *str)
{
    char buffer[16];
    strcpy(buffer,str);
}

void main()
{
    char large_string[256];
    int i;
 
    for( i = 0; i < 255; i++)
        large_string[i] = 'A';
 
    function(large_string);
}
```

&emsp;&emsp;上面的函数 `function()` 存在缓冲区溢出的错误，他直接用了 `strcpy()` 函数，没用用 `strncpy()` 函数进行拷贝字节的大小判断。如果执行上边的程序将会出现段错误。当我们调用函数时，栈的情况类似于下面这种：

![stack_funciton_2](http://ww3.sinaimg.cn/large/9d15fcebgw1erom17rtlfj20hg03s3yf.jpg)

&emsp;&emsp;这里为什么会出现段错误呢？很简单，因为我们要把256个字节的’A’拷贝到16个字节大小的buffer中，所以出现了覆盖的情况，从上图中可以看到sfp、ret、*str都被覆盖成了’A’,而’A’的ASIC码是0x41，所以ret中是0x41414141,这一地址超过了程序的地址范围，所以是非法访问，会出现段错误。所以缓冲区溢出可以让我们修改函数的返回地址，根据这一点我们可以改变程序的执行流程。我们再回看一下例子1中当调用function时栈的情况：

![stack_funciton_3](http://ww3.sinaimg.cn/large/9d15fcebgw1erom1nwaepj20h603pt8n.jpg)

我们试着去修改上面的ret部分让他执行任意的代码，修改后的代码如下：

```C
//example3.c:
void function(int a, int b, int c)
{
    char buffer1[5];
    char buffer2[10];
    int *ret;
    ret = buffer1 + 12;
    (*ret) += 8;
}

void main()
{
    int x;
    x = 0;
    function(1,2,3);
    x = 1;
    printf("%d\n",x);
}
```

例子中我们用buffer1加上12得到ret的值，函数返回值即存储在ret中，然后让ret的内容加8跳过x=1赋值语句直接执行printf函数。那么我们是怎么知道加8的呢？借用gdb：

![gdb1](http://ww2.sinaimg.cn/large/9d15fcebgw1erom229z6vj20i10c1myd.jpg)

可以看到之前的 `ret` 值是 `0x80004a8` ，我们跳过赋值语句直接执行 `0x80004b2` ，两者相减得8（应该是10）.

#### 四.shell code

&emsp;&emsp;所以，现在我们可以修改函数的返回地址，改变程序执行的流程，那么我们要执行什么样的程序呢？在大部分情况下我们仅仅想要启动一个shell，然后我们可以在这个shell上随便执行我们的命令。但是如果程序中没有这样的代码怎么办？当然是我们自己开发，但是这段代码要放在那里呢？答案是溢出的缓冲区，并且要重写ret的值让其指向这段有我们代码的缓冲区。就下面的情况，栈顶在 `0xFF` ，我们想要执行缓冲区中的代码S：

![code1](http://ww1.sinaimg.cn/large/9d15fcebgw1eropzvh3z6j20he07iq36.jpg)

这段启动shell的代码被称为shell code，在C中shell code类似这样：

```C
//shellcode.c:
#include stdio.h
void main()
{
    char *name[2];
    name[0] = "/bin/sh";
    name[1] = NULL;
    execve(name[0], name, NULL);
}
```

为了看一下他在汇编中是怎么实现的，我们编译之后用gdb跟踪观察一下，注意一定要用-static参数，否则在实际的代码中就不会包括系统调用execve,它会在程序加载的时候以c动态库的形式链接。

![gdb2](http://ww1.sinaimg.cn/large/9d15fcebgw1eroq0c9v2hj20hj0aut9x.jpg)
![gdb3](http://ww1.sinaimg.cn/large/9d15fcebgw1eroq0jh52fj20h10bkt9z.jpg)

总结之后我们发现我们只需要做下面几件事就行：

    a) 把以NULL结尾的字串”/bin/sh”放到内存某处.   
    b) 把字串”/bin/sh”的地址放到内存某处, 后面跟一个空的长字(null long word).    
    c) 把0xb放到寄存器EAX中.    
    d) 把字串”/bin/sh”的地址放到寄存器EBX中.    
    e) 把字串”/bin/sh”地址的地址放到寄存器ECX中.(注: 原文d和e步骤把EBX和ECX弄反了)    
    f) 把空长字的地址放到寄存器EDX中.    
    g) 执行指令 int x80.

但是如果当execve()执行失败了那么程序就会继续执行，出现core dump的情况，所以我们需要加入系统调用exit().
经过查看汇编代码，exit()所要做的事情如下：

    a)把0x1放到寄存器EAX中
    b)把0x0放到寄存器EBX中
    c)执行指令int x80

将两部分结合之后翻译成汇编指令如下：

![code2](http://ww4.sinaimg.cn/large/9d15fcebgw1eroq0u58udj209d05wjri.jpg)

将汇编指令翻译成二进制的代码，并且将其中\x00的代码进行替换，于是得到下面的程序：

![code3](http://ww3.sinaimg.cn/large/9d15fcebgw1eroq12i87bj20he066dg1.jpg)

运行程序可以看到轻松得到shell。

#### 五.攻击程序
现在我们有了一个shellcode，当我们发现有缓冲区溢出的程序时，就可以将这段shell代码当作字符串输入程序中，从而得到shell。但是这里存在一个问题就是返回地址（ret）必须是一个绝对的地址，而且一定要指向shellcode的起始位置，这就比较麻烦。而在Alpha One的文章中也没提到很好的办法，所以我们只能用猜的方法。但是有一个提高命中率的方法在文章中提到，例如我们的缓冲区有512字节，而shellcode只有20字节，那么我们可以在shellcode之前全部填充上NOP指令，这样只要ret指向了NOP就可以执行到shellconde。可以见下图：

![stack_function5](http://ww4.sinaimg.cn/large/9d15fcebgw1eroq1xsp1mj20hx04w74g.jpg)

N代表NOP指令，S代表shellcode。如上图，只要ret指向了0x/D8 – 0x/E3 shellcode都可以被执行。

***
(EOF)
