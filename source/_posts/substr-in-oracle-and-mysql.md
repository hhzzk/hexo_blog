title: "oracle和mysql中substr的区别"
date: 2015-02-04 00:46:21
tags: 
  - mysql 
  - oracle 
  - 数据库
  - substr
categories:
  - 原创
thumbnailImagePosition: left
thumbnailImage: http://7xrt06.com1.z0.glb.clouddn.com/16-3-14/28146611.jpg
---

本文主要讲解oracle和mysql中substr实现上的一些区别。
<!--more-->
1.&emsp;mysql中支持substring和substr，并且两者的功能完全一样。oracle中只支持substr。
2.&emsp;mysql中支持的格式如下：
```C
    SUBSTRING(str,pos)
    SUBSTRING(str FROM pos) 
    SUBSTRING(str,pos,len)
    SUBSTRING(str FROMpos FOR len)
    SUBSTR(str,pos)
    SUBSTR(str FROM pos) 
    SUBSTR(str,pos,len) 
    SUBSTR(str FROM pos FOR len)
```
oracle中支持的格式如下：
```c
SUBSTR( string, start_position, [ length ] )
```
3.&emsp;对于mysql，如果pos为0，则返回为空，如果pos为1，则从第一个字符开始，如下图：
![mysql_substr_0](http://ww4.sinaimg.cn/large/9d15fcebgw1ermr34q7amj206q038q31.jpg) &emsp; ![mysql_substr_1](http://ww1.sinaimg.cn/large/9d15fcebgw1ernbk4gzw7j206v039jrk.jpg)
4.&emsp;对于oracle，start_position为0时当做1来处理，也就是start_position为0或者为1的结果相同。
5.&emsp;如果length小于1，mysql和oracle都会返回空。
6.&emsp;oracle官方note:
> * If position is 0, then it is treated as 1.
> * If position is positive, then Oracle Database counts from the beginning of char to find the first character.
> * If position is negative, then Oracle counts backward from the end of char.
> * If substring_length is omitted, then Oracle returns all characters to the end of char. If substring_length is less than 1, then Oracle returns null.

7.&emsp;mysql举例：
```shell
mysql> SELECT SUBSTRING('Quadratically',5);
 -> 'ratically'
mysql> SELECT SUBSTRING('foobarbar' FROM 4);
-> 'barbar'
mysql> SELECT SUBSTRING('Quadratically',5,6);
 -> 'ratica'
mysql> SELECT SUBSTRING('Sakila', -3);
 -> 'ila'
mysql> SELECT SUBSTRING('Sakila', -5, 3);
 -> 'aki'
mysql> SELECT SUBSTRING('Sakila' FROM -4 FOR 2);
-> 'ki'
```
8.&emsp;oracle举例：
```shell
SELECT SUBSTR('ABCDEFG',3,4) "Substring"
     FROM DUAL;
Substring
---------
CDEF

SELECT SUBSTR('ABCDEFG',-5,4) "Substring"
     FROM DUAL;
Substring
---------
CDEF
```
9.&emsp;官方解释：
 + Mysql官方对substr的解释: [链接](http://dev.mysql.com/doc/refman/5.0/en/string-functions.html#function_substring)
 + Oracle官方对substr的解释: [链接](http://docs.oracle.com/cd/B19306_01/server.102/b14200/functions162.htm)

10.&emsp;最后,一张图搞懂mysql中`substring`.(来源:[w3resource](http://www.w3resource.com/))
![](http://ww1.sinaimg.cn/large/9d15fcebgw1ernbzey82ig20g10nv757.gif)
***
(完)
