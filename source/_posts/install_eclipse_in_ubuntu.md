title: "如何在Ubuntu上安装Eclipse 4.4 (Luna)"
date: 2014-12-7 15:50:57
tags:
  - ubuntu
  - Eclipse安装
  - Linux命令
categories:
  - 原创
  - Linux命令系列
thumbnailImagePosition: left
thumbnailImage: http://7xrt06.com1.z0.glb.clouddn.com/16-3-15/56095188.jpg
---

在 Ubuntu 上安装 Eclipse 并且固定在启动栏中
<!-- excerpt -->

#### 第一步：下载

&emsp;&emsp;在Eclipse官网下载安装包：[链接](http://www.eclipse.org/downloads/)，依次点击：`“Linux” 32-bit 或者 64-bit ⇒ “Eclipse IDE for Java Developers” (或者 “Eclipse Standard|Classic”) for Java SE program development; 或者 “Eclipse IDE for Java EE Developers” for developing webapps. 下载之后会得到一个类似于“eclipse-jee-luna-R-linux-gtk-x86_64.tar.gz“的文件。

#### 第二步：安装
&emsp;&emsp;依次执行以下命令：

```shell
// Unzip the tarball into /usr/local
$ cd /usr/local
$ sudo tar xzvf ~/Downloads/eclipse-jee-luna-R-linux-gtk-x86_64.tar.gz
      // Extract the downloaded package
      // x: extract, z: for unzipping gz, v: verbose, f: filename
      // You can also unzip in "File Explorer" by double-clicking the tarball.
 
// Change ownership
$ cd /usr/local
$ sudo chown -R your-username:your-groupname eclipse
      // Change ownership to your chosen username and groupname
      // -R recursive
 
// Set up a symlink
$ cd /usr/bin
$ sudo ln -s /usr/local/eclipse/eclipse
      // Make a symlink in /usr/bin, which is in the PATH.
$ ls -ld eclipse
lrwxrwxrwx 1 root root 26 Aug 30 11:53 eclipse -> /usr/local/eclipse/eclipse
$ which eclipse
/usr/bin/eclipse
$ whereis eclipse
eclipse: /usr/bin/eclipse /usr/bin/X11/eclipse /usr/local/eclipse
```

安装完成，要想运行eclipse可以打开 `“/usr/local/eclipse“`目录，点击eclipse图标或者打开一个终端输入eclipse。

#### 第三步：启动栏
&emsp;&emsp;要想将eclipse放置在启动栏上，首先在桌面创建文件eclipse.desktop，然后输入以下内容：

```shell
[Desktop Entry]
Name=Eclipse 
Type=Application
Exec=eclipse
Terminal=false
Icon=/usr/local/eclipse/icon.xpm
Comment=Integrated Development Environment
NoDisplay=false
Categories=Development;IDE;
Name[en]=Eclipse
```

保存之后文件就会变成eclipse图标，右击此图标，选择 `lock to launcher`。

***
(EOF)

