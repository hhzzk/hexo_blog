title: "Linux下删除文件行尾的^M"
date: 2014-12-24 19:50:57
tags:
  - Vim
  - Linux命令
  - dos2unix
categories:
  - 原创
  - Linux命令系列
thumbnailImagePosition: left
thumbnailImage: http://7xrt06.com1.z0.glb.clouddn.com/16-3-13/8340578.jpg
---

由于不同的操作系统采用了不同的换行符，所以当 windows 的文件 copy 到 Linux 之后会发现行尾多出 `^M` 。
<!-- excerpt -->

做编译原理实验时，在vim中打开实验用到的文件发现行尾都带着 `^M` 。google之后发现原来这是因为不同的操作系统采用了不同的换行符造成的。在不同的系统上换行符分别如下：

| OS | sign |
| :-----: |:-----:|
|unix| \n |
|dos|\r\n|
|windows|\r\n|
|mac|\r|

实际上 `\r` 代表着回车（ASCII 码为0D）， `\n` 表示换行符（ASCII码为0A）。
所以当把一个文件从一个系统转移到另外的系统时，就会有换行符方面的问题。
因为在 windows 中用 `\r\n` 表示换行，而在 linux 中用 `\n` 表示，所以这里会多出一个 `\r` 。


 **解决方法**

##### 方法一 :
在 Vim 中可以使用替换命令进行删除，如下：
```
:%s/\r//g
```
或者使用下面的命令：
```
:%s/^M//g 
```
其中 `^M` 是由 `Ctrl + v` 和 `Ctrl + M` 生成的，不要直接用 `^` 和 `M`  。



##### 方法二 :
另外,在 linux 下也可以使用 `dos2unix` 这个工具将 windows 或者 dos 格式的文件转换成 linux 格式的文件,首先需要安装 `dos2unix` 这个软件,在 ubuntu 下直接运行:
```shell
sudo apt-get install dos2unix
```
安装成功后直接对文件进行操作:
```shell
dos2unix filename
```
***
(EOF)
