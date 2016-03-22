title: "技术日志: linux防火墙,oracle安装"
date: 2015-03-12 15:50:57
tags:
  - ubuntu
  - 防火墙
  - Linux命令
categories:
  - 原创
  - 技术日志
thumbnailImagePosition: left
thumbnailImage: http://7xrt06.com1.z0.glb.clouddn.com/16-3-20/76370097.jpg
---

Linux防火墙相关命令，在redhat上安装oracle。
<!-- excerpt -->

#### 1.linux防火墙方面的命令
机器重启后所做配置会失效：
```shell
service iptables stop
service iptables start 
service iptables restart
```
机器重启后所做配置不会失效：
```shell
chkconfig iptables on
chkconfig iptables off 
```

#### 2. 在redhat上安装oracle 11.2 相关：
参考教程： [链接](http://www.cnblogs.com/zhangyongli2011/archive/2012/04/04/2431953.html)
必须要安装全部的rpm包，虽然系统是64位的，oracle的版本也是64位的，但是有些包 32位和64位的 必须都要安装。
下面几条命令用来查看、启动、停止监听服务
```shell
lsnrctl status
lsnrctl start
lsnrctl stop
```
***
(EOF)
