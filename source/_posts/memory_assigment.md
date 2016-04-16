title: "内存分配，任意字节对齐"
date: 2015-04-14 16:54:57
tags:
  - 面试题
  - 算法
  - 内存分配
categories:
  - 原创
  - 算法系列
thumbnailImagePosition: left
thumbnailImage: http://7xrt06.com1.z0.glb.clouddn.com/16-4-13/33672995.jpg
---

实现可以对齐到任意字节的malloc函数。
<!-- excerpt -->

malloc的对齐策略遵循以下两个原则：
1.必须是2的幂
2.必须是(void *)的整数倍。
具体的实现方法可以参考这篇[博客](http://www.cnblogs.com/Creator/archive/2012/04/05/2433386.html)
但在很多笔试面试题中经常要求应聘者自己实现malloc函数，并且要求可以对齐到任意字节（既可以被任意字节整除）

以下为实现方法：
alignment表示要对齐的字节。
```c
void * aligned_malloc (int size, int alignment) 
{
  // 利用系统的malloc分配，但是每次都多分配alignment大小的空间
  void* ptr = malloc(size + alignment);

  if (ptr)
    {
      // 调整地址，使其对齐到alignment。原理就是当前分配的地址加上一定的偏移量。
      // 注意如果分配的地址正好对齐到aligment上，也要进行调整，正好加上aligment大小的偏移量
      void* aligned = (void*)(((long)ptr + alignment) & ~(alignment - 1));
      // 保存实际的分配地址，释放内存空间时使用。 
      ((void**)aligned)[-1] = ptr;

      return aligned;
    }
  else
    return NULL;
}
void *aligned_free(void  *paligned)
{
      // 释放内存，注意是使用 delete[]
      delete [ ]paligned;
}
```
delete 和 delete[] 的区别
delete和delete[] 用来释放内存，调用析构函数。
如果只是释放堆上的内存，两者好似没有区别的，无论是new还是new[]申请的内存空间，用delete都可以释放。
主要的区别是调用析构函数，delete只会调用第一个对象的析构函数，而delete[]则会调用所有由new[]产出的对象的析构函数。
另外，delete p 是删除释放一个单元，delete [] p 是释放 多个单元，具体的数据目是查系统的分配表得到的
更详细的分析看[这里](http://www.cnblogs.com/hazir/p/new_and_delete.html)

***
(EOF)
