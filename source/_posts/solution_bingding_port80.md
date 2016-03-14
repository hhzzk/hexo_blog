title: "80端口被占用的解决办法"
date: 2014-10-25 12:46:21
tags: 
  - fuser
  - Linux命令 
  - 80端口
categories:
  - 原创
  - Linux命令系列
thumbnailImagePosition: left
thumbnailImage: http://7xrt06.com1.z0.glb.clouddn.com/16-3-13/8340578.jpg
---

很多时候80端口被占用，但是通过 netstat 无法查找对应进程所以无法 kill 的情况，可以试试文章中的方法。
<!-- excerpt -->

&emsp;&emsp;做信息安全实验的时候80端口总是无法绑定，一直在报绑定错误。谷歌之后大部分的解决办法都是 `netstat -apn | grep 80`  ，找到相应的进程号，然后kill掉。但试过之后显示如下：

    tcp 0 0 0.0.0.0:80 0.0.0.0:* LISTEN

可以看到80端口确实被占用，但是没有显示具体哪一个进程正在占用，所以无法kill掉。继续搜索发现有一条命令是这样: 

    sudo fuser -k 80/tcp
试过之后果然可以。

Tips: 

- 小于1024的端口是专用端口，所以如果要想绑定到自己的程序上必须获取最高权限，Ubuntu下使用sudo。
- fuser用途：查询给定文件或目录的用户或进程信息。
***
(EOF)


