title: "ubuntu时间设置相关总结"
date: 2015-01-14 19:19:45
tags:
  - ubuntu
  - 系统设置
  - ubuntu时间设置
  - 同步网络时间
  - Linux命令
categories:
  - 原创
  - Linux命令系列
thumbnailImagePosition: left
thumbnailImage: http://7xrt06.com1.z0.glb.clouddn.com/16-3-13/35334370.jpg
---
ubuntu中时间的设置.包括CST到UTC的修改,系统时间与BIOS时间的同步设置,同步网络时间的设置等.
<!--more-->
### 1.&ensp;名词解释：
* `GMT`：格林威治标准时间(Greenwich Mean Time，简称G.M.T.)；
* `UTC`：世界协调时间(Universal Time Coordinated,UTC)
* `CST` ：中国沿海时间(北京时间)（China Standard Time UTC+8:00）

### 2.&ensp;修改CST到UTC
在linux中查看时间执行`date`命令，如果使用的是`CST`格式，则会比正常时间晚8个小时，需要修改成`UTC`格式。修改方式如下:
1. &ensp;`sudo dpkg-reconfigure tzdata` ;
2. &ensp;在弹出的界面中选择”etc” ;
3. &ensp;在下一个页面选择“utc”,之后回车 。

以上命令实际上修改的是/etc/timezone 和 /etc/localtime。

### 3.系统时间和BIOS时间
linux中存在两个时间，系统时间和BIOS时间。当系统启动时会读取BIOS时间，系统关闭时会把当前时间写回BIOS。
* 查看系统时间：`dat` 
* 查看BIOS时间：`sudo hwclock –show`
* 系统时间设置： `sudo date MMDDhhmmCCYY.ss`（月、日、小时、分钟、年、秒）
* 同步BIOS时间：`sudo hwclock –systohc` 或者 `sudo hwclock –systohc –utc`

### 4.同步网络时间
`sudo ntpdate cn.pool.ntp.org`
tips：需要先安装ntpdate（sudo atp-get install ntpdate）
***
(EOF)
