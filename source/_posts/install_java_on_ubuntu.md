title: "如何在Ubuntu上安装Oracle Java 8"
date: 2014-11-27 19:50:57
tags:
  - ubuntu
  - Java安装
  - Linux命令
  - JDK
categories:
  - 原创
  - Linux命令系列
thumbnailImagePosition: left
thumbnailImage: http://7xrt06.com1.z0.glb.clouddn.com/16-3-13/8340578.jpg
---

在Ubuntu上安装Oracle Java 8 有两种方法，一种是通过PPA，一种是通过官方安装文件。
<!--more-->
### 通过PPA安装：

#### 第一步：安装Java 8 （JDK 8）
首先在系统中添加 `webupd8team java PPA` 仓库，命令如下：
```shell
$ sudo add-apt-repository ppa:webupd8team/java
$ sudo apt-get update
$ sudo apt-get install oracle-java8-installer
```
这个过程可能要花费很长的时间，一定确保网络连接正常。 

#### 第二步：确认Java的版本
```shell
$ java -version
java version "1.8.0_25"
Java(TM) SE Runtime Environment (build 1.8.0_25-b17)
Java HotSpot(TM) 64-Bit Server VM (build 25.25-b02, mixed mode)
```
#### 第三步：设置 Java 环境变量
```shell
$ sudo apt-get install oracle-java8-set-default
```
ok，通过PPA方式大功告成！

### 通过源码安装
安装之前：检测JDK是否已经安装
在终端中输入以下命令：
```shell
$ javac -version
```
如果出现了类似“javac 1.x.x_xx“的信息，说明JDK已经被安装了，如果要卸载可以执行下面的命令：
```shell
$ sudo apt-get purge openjdk-\*
```

#### 第一步：下载JDK
到[Oracle官网]( http://www.oracle.com/technetwork/java/javase/downloads/index.html)，依次点击 `Java SE 8u{xx}` ⇒  `JDK ` ⇒  `Download `⇒   `Accept License Agreement ⇒ Select Linux x86 (for 32-bit system) or Linux x64 (for 64-bit system) “tar.gz“ ` 。（检测操作系统版本： `Settings ⇒ Details` 或者键入命令`file /sbin/init`）

#### 第二部：解压
我们将把JDK安装在 `/usr/local/java` (也可以安装在Ubuntu的默认 JDK 目录  `/usr/lib/jvm` 下)，首先在 `usr/local` 下创建目录java，命令如下：
```shell
$ cd /usr/local
$ sudo mkdir java
```
解压（注意自己的文件名）：
```shell
$ cd /usr/local/java
$ sudo tar xzvf ~/Downloads/jdk-8u{xx}-linux-x64.tar.gz
       // x: extract, z: for unzipping gz, v: verbose, f: filename
```
这样JDK就被解压在了 `/usr/local/java/jdk1.8.0_{xx}` 目录下。

#### 第三步：安装
分别执行以下的命令进行安装：
```shell
// Setup the location of java, javac and javaws
$ sudo update-alternatives --install "/usr/bin/java" "java" "/usr/local/java/jdk1.8.0_{xx}/jre/bin/java" 1
      // --install symlink name path priority
$ sudo update-alternatives --install "/usr/bin/javac" "javac" "/usr/local/java/jdk1.8.0_{xx}/bin/javac" 1
$ sudo update-alternatives --install "/usr/bin/javaws" "javaws" "/usr/local/java/jdk1.8.0_{xx}/jre/bin/javaws" 1
 
// Use this Oracle JDK/JRE as the default
$ sudo update-alternatives --set java /usr/local/java/jdk1.8.0_{xx}/jre/bin/java
      // --set name path
$ sudo update-alternatives --set javac /usr/local/java/jdk1.8.0_{xx}/bin/javac
$ sudo update-alternatives --set javaws /usr/local/java/jdk1.8.0_{xx}/jre/bin/javaws
```
通过以下命令查看是否设置成功：
```shell
$ cd /usr/bin
$ ls -ld java*
lrwxrwxrwx 1 root root 22 Mar 31 20:41 java -> /etc/alternatives/java
lrwxrwxrwx 1 root root 23 Mar 31 20:42 javac -> /etc/alternatives/javac
lrwxrwxrwx 1 root root 24 Mar 31 20:42 javaws -> /etc/alternatives/javaws
 
$ cd /etc/alternatives
$ ls -ld java*
lrwxrwxrwx 1 root root 40 Aug 29 18:18 java -> /usr/local/java/jdk1.8.0_20/jre/bin/java
lrwxrwxrwx 1 root root 37 Aug 29 18:18 javac -> /usr/local/java/jdk1.8.0_20/bin/javac
lrwxrwxrwx 1 root root 42 Aug 29 18:19 javaws -> /usr/local/java/jdk1.8.0_20/jre/bin/javaws
```

#### 第四步：确认安装
使用以下命令确认是否安装成功：
```shell
// Show the Java Compiler (javac) version
$ javac -version
javac 1.8.0_20
 
// Show the Java Runtime (java) version
$ java -version
java version "1.8.0_20"
Java(TM) SE Runtime Environment (build 1.8.0_20-b26)
Java HotSpot(TM) 64-Bit Server VM (build 25.20-b23, mixed mode)
 
// Show the location of javac and java
$ which javac
/usr/bin/javac
$ which java
/usr/bin/java
```
ok，安装完成！
***
(EOF)


