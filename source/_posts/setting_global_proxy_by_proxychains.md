title: "利用proxychains v4在ubuntu下设置全局代理"
date: 2015-06-10 19:50:57
tags:
  - Linux命令
  - proxychains
categories:
  - 原创
  - Linux命令系列
thumbnailImagePosition: left
thumbnailImage: http://7xrt06.com1.z0.glb.clouddn.com/16-3-13/8340578.jpg
---

利用`proxychains v4`在ubuntu下设置全局代理以及解决 `ERROR: ld.so: object ‘libproxychains.so.3’ from LD_PRELOAD cannot be preloaded: ignored` 的问题
<!-- excerpt -->

### 利用proxychains v4在ubuntu下设置全局代理
#### 1. 如果之前安装过proxychains，需要先删除
```shell
    sudo apt-get remove proxychains
```
#### 2. 安装 proxychains v4
```shell
git clone https://github.com/rofl0r/proxychains-ng
cd proxychains-ng
```

#### 3. 编译

```shell
./configure –prefix=/usr –sysconfdir=/etc
sudo make
sudo make install
```
#### 4. 生成配置文件：
```shell
sudo make install-config
```
#### 5. 修改配置文件：
```shell
sudo vim /usr/local/etc/proxychains.conf
// 将[ProxyList] 后面的部分设置为自己的代理设置即可 支持多个轮询 例如：
socks5 127.0.0.1 1080
```
#### 6. 使用方法：
```shell
proxychains4 应用名
例如：
wget google.com  // 无法获取index.html
proxychains4 wget googel.com  // 可以获得index.html
```
但是在使用此方法执行 `ssh` ,经常出现连接成功后在很短的时间内断开的现象，解决方法如下：
第一种方案在客户端设置：
```shell
sudo vim /etc/ssh/ssh_config

// 添加下面语句，这样客户端会每个10s发送一个keepAive请求。
ServerAliveInterval 10
```

第二种方案在服务器端设置：
```shell
sudo vim /etc/ssh/ssh_config

// 添加下面的语句
ClientAliveInterval 60
```
第二种方案需要重启`ssh`服务（`sudo /etc/init.d/ssh restart`）。

安装了此版本的 `proxychains` 之后，`ERROR: ld.so: object ‘libproxychains.so.3’ from LD_PRELOAD cannot be preloaded: ignored`这个错误也会解决。

---
（EOF）
