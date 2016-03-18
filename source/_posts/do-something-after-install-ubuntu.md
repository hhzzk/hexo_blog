title: "安装ubuntu后需要做的一些事"
date: 2015-2-20 15:50:57
tags:
  - ubuntu
  - Linux命令
categories:
  - 原创
  - Linux命令系列
thumbnailImagePosition: left
thumbnailImage: http://7xrt06.com1.z0.glb.clouddn.com/16-3-18/55460490.jpg
---

之前一直把Ubuntu装在虚拟机上，但是卡的要死，所以刚刚装了双系统，刚刚装的ubuntu难免有一些东西要捯饬一下。 
<!-- more -->
#### 把中文环境换成英文环境 
可能是安装的时候选成了中文，导致系统环境都是中文的，用着很不爽。 
修改办法： 
##### 第一步,安装en_US.utf8
终端下运行 `locale -a` 查看有没有安装 `en_US.utf8` ， 如果没有，可以使用命令 `sudo locale-gen en_US.UTF-8` 进行安装 
##### 第二步,修改locale文件 
在特权模式下运行下面的命令将locale打开 
```shell
sudo vi /etc/default/locale 
```
可以看到如下的内容： 

>LANG="zh_US.UTF-8" 
>LANGUAGE="zh_US:zh" 

我们只要将 `zh_US.UTF-8` 改为 `en_US.UTF-8` ，zh改为en即可，如下： 

>LANG="en_US.UTF-8" 
>LANGUAGE="en_US:en" 

修改完之后，保存退出。 
重启电脑或者执行下面的命令使配置生效： 

```shell
source /etc/default/locale 
```

注意：系统环境由中文改为英文之后，当重启系统之后，系统会弹出对话框，询问是否将home目录下默认的文件夹（下载、桌面、模板等等）改为英文。如果选择修改那么你必须再次重启电脑，否则桌面等是无法使用了。例如，我改成英文之后想在桌面创建一个临时文件，但系统提示我找不到“桌面”。 


#### 安装chrome 
之前一直用chrome，所有的书签也都保存在google账号中，并且也要用它翻墙，所以需要安装chrome。 
##### 1. 下载安装包
执行下面的命令，不需要翻墙就能下载 

- a. 32位稳定版
```shell
wget https://dl.google.com/linux/direct/google-chrome-stable_current_i386.deb) 
```
- b. 32位Unstable版
```shell
wget https://dl.google.com/linux/direct/google-chrome-unstable_current_i386.deb)
```
- c. 64位稳定版： 
```shell
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb 
```
- d. 64位Unstable版： 
```shell
wget https://dl.google.com/linux/direct/google-chrome-unstable_current_amd64.deb
```

选择32位还是64位就要看你的系统了。至于稳定版还是Unstable版，网上是这样讲的“如果你追求稳定的话，最好选择稳定版，如果你喜欢尝试新功能并追求最好的性能，推荐安装Unstable版，而且似乎在Ubuntu下，Unstable版的Chrome中的字体显示也要比Beta好看一些”。因为之前一直使用稳定版，这次想要尝试一下Unstable版，所以安装了64位的Unstable版，确实如别人讲的那样，字体好看了并且感觉更快了些。 
##### 2. 更新升级： 
```shell
sudo apt-get update 
```

##### 3. 安装： 
进入到安装包的目录，执行下面的命令： 
sudo dpkg -i google-chrome-unstable_current_amd64.deb 
这一步有可能会失败，那是因为缺少响应的依赖包，需要执行下面的命令安装： 
sudo apt-get install -f 
安装好依赖包之后再次执行安装命令。 
至此chrome就安装好了，你可以在系统中搜索到chrome，然后点击打开并且将其固定到launcher上。 



#### 安装搜狗输入法 
中文输入法是必须的，相对来说搜狗做的应该是比较好的。 
安装过程参考[这里](http://jingyan.baidu.com/article/ad310e80ae6d971849f49ed3.html)
总结如下： 
- a）官网下载安装包，点击安装 
- b）终端下输入 `im-config` --> `点击OK` --> `点击yes` --> `选择fcitx,点击OK` --> `点击OK` --> `重启电脑` 
- c）终端下输入 `fcitx-config-gtk` --> 点击对话框左下角的（+）按钮 --> 取消`Only Show Current Language` --> 在输入框中输入sogou，选中点击OK 

*tips：在最下方的是默认输入法。*


#### 改变ubuntu主题
对Ubuntu默认的主题实在无爱,还是比较喜欢numix,安装过程参见[这里](http://itsfoss.com/how-to-install-themes-in-ubuntu-13-10/)
安装过程总结一下就是下面几条命令:
```shell
sudo apt-get install unity-tweak-tool
sudo add-apt-repository ppa:numix/ppa
sudo apt-get update
sudo apt-get install numix-icon-theme-shine numix-icon-theme-circle
```

然后打开 `unity-tweek-tool`, 选择 `Theme`, 选择 `numix` 即可.

#### 修改配置
一直纠结于Vim的配色问题,直到找到[spf13-vim](https://github.com/spf13/spf13-vim).
终端配色: 文字颜色设为#708284，背景颜色设为#07242E
终端字体: courier 10 pitch 12 

#### 安装nodejs
nodejs依赖python and g++, 必须提前安装好. nodejs有两种安装方式,第二种的安装方式不容易成功,会出现很多奇怪的问题(主要还是权限的问题),例如[这里](http://stackoverflow.com/questions/16151018/npm-throws-error-without-sudo)讨论的情况我就遇到过,最好利用第一种的nvm进行安装. 

##### 第一种
a. 安装nvm 
```shell
curl https://raw.githubusercontent.com/creationix/nvm/v0.17.2/install.sh | bash 
```

安装完成后,重启终端 

b. 查看nodejs版本信息,选择想要安装的版本 
```shell
nvm ls-remote 
```

c. 安装nodejs 
```shell
nvm install 0.12.2 
```

##### 第二种 
添加源之后使用apt-get安装
```shell
sudo add-apt-repository ppa:chris-lea/node.js 
sudo apt-get update 
sudo apt-get install nodejs 
sudo apt-get install npm 
```

查看版本 
```shell
node -v 
npm -v 
```
此种安装方法有可能不是最新的版本所以需要升级:
```shell
sudo npm cache clean -f
sudo npm update –g
sudo npm install -g n 
sudo n latest 
```
***
(EOF)


