title: "Linux程序运行时在前后台切换"
date: 2015-04-6 09:30:15
tags:
  - Linux命令
categories:
  - 原创
  - Linux命令系列
thumbnailImagePosition: left
thumbnailImage: http://7xrt06.com1.z0.glb.clouddn.com/16-3-13/8340578.jpg
---

如何让Linux程序在后台执行，如何将后台执行的程序调入前台执行。
<!-- excerpt -->

1、在Linux终端执行命令时，在命令末尾加上符号 &，就可以让程序在后台运行, 例如,在我的ubuntu上运行 `hexo s` ,可以看到如下的效果,
```
king@king:~/blog$ hexo s &
[1] 26421
king@king:~/blog$ INFO  Hexo is running at http://0.0.0.0:4000/. Press Ctrl+C to stop.

king@king:~/blog$ 
king@king:~/blog$ 
```
执行命令之后,程序进入后台运行,并且返回进程号.

2、如果程序正在前台运行，可以使用 `Ctrl+z` 选项把程序暂停，然后用 `bg %[number]` 命令把这个程序放到后台运行
```
king@king:~/blog$ hexo s
INFO  Hexo is running at http://0.0.0.0:4000/. Press Ctrl+C to stop.
^Z
[1]+  Stopped                 hexo s
king@king:~/blog$ bg %1
[1]+ hexo s &
king@king:~/blog$ 
```
`Ctrl+z` 之后会返回响应的number.

3、对于所有运行的程序，我们可以用jobs –l 指令查看
```
king@king:~/blog$ jobs -l
[1]+ 27061 Running                 hexo s &
king@king:~/blog$
```

4、也可以用 fg %[number] 指令把一个程序掉到前台运行
```
king@king:~/blog$ fg %1
hexo s
```

5、如果在退出帐户/关闭终端之后想要继续运行相应的进程,需要用到nohup命令
```
king@king:~/blog$ nohup hexo s &
[1] 31260
king@king:~/blog$ nohup: ignoring input and appending output to ‘nohup.out’

king@king:~/blog$ 
```
并且他会把程序的输出重定向到nohup.out文件中

***
(EOF)

