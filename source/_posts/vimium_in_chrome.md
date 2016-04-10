title: "Vimium: 像Vim一样操作Chrome"
date: 2015-08-15 20:05:05
tags:
  - Vimium
categories:
  - 原创
  - 提高效率
thumbnailImagePosition: left
thumbnailImage: http://7xrt06.com1.z0.glb.clouddn.com/16-4-10/45563707.jpg
---

介绍一个Chrome插件Vimium，它可以让我们像Vim一样操作Chrome。
<!-- excerpt -->
Vim的设计初衷是操作者不必将手移开键盘，所有的操作都可以由键盘完成，这也是Vim高效从而被程序员钟爱的原因之一。Chrome应该是大部分程序员的最爱，简介、快速、调试方便。如果能像Vim一样操作Chrome，不用在键盘鼠标之间来回切换，那效率肯定会大大的提高。
这里介绍一个Chrome插件Vimium，它可以让我们像Vim一样操作Chrome。
插件地址：[Google web store](https://chrome.google.com/webstore/detail/vimium/dbepggeogbaibhgnhhndojpepiihcmeb?hl=en)
如果无法翻墙访问，可以到 github 下载 [vimium](https://github.com/philc/vimium/releases)
这里介绍几个常用的命令，更详细的操作可以到 [github](https://github.com/philc/vimium) 查看或者看这个教学[视频](https://www.youtube.com/watch?v=t67Sn0RGK54)
### 常用基本操作
<pre>
?       show the help dialog for a list of all available keys
h       scroll left
j       scroll down
k       scroll up
l       scroll right
gg      scroll to top of the page
G       scroll to bottom of the page
d       scroll down half a page
u       scroll up half a page
f       open a link in the current tab
F       open a link in a new tab
r       reload
</pre>

### 查找操作
<pre>
/       enter find mode
          -- type your search query and hit enter to search, or Esc to cancel
n       cycle forward to the next find match
N       cycle backward to the previous find match
</pre>

### Tab 相关操作
<pre>
J, gT   go one tab left
K, gt   go one tab right
g0      go to the first tab
g$      go to the last tab
t       create tab

x       close current tab
X       restore closed tab (i.e. unwind the 'x' command)

</pre>

***
(EOF)


