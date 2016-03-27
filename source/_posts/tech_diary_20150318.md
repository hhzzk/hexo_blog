title: "技术日志:Linux命令 more head tail cat"
date: 2015-03-18 21:30:15
tags:
  - Linux命令
categories:
  - 原创
  - 技术日志
thumbnailImagePosition: left
thumbnailImage: http://7xrt06.com1.z0.glb.clouddn.com/16-3-20/76370097.jpg
---
Linux文件显示命令总结
<!-- excerpt -->
#### Linux命令more
`more` 命令，功能类似 `cat` ， `cat`命令是整个文件的内容从上到下显示在屏幕上。`more` 会以一页一页的显示方便使用者逐页阅读，而最基本的指令就是按空白键（space）就往下一页显示，按 b 键就会往回（back）一页显示，而且还有搜寻字串的功能 。`more` 命令从前向后读取文件，因此在启动时就加载整个文件。
##### 1．命令格式：

    more [-dlfpcsu ] [-num ] [+/ pattern] [+ linenum] [file ... ] 

##### 2．命令功能：
`more` 命令和 `cat` 的功能一样都是查看文件里的内容，但有所不同的是 `more` 可以按页来查看文件的内容，还支持直接跳转行等功能。
##### 3．命令参数：
```
+n      从笫n行开始显示
-n       定义屏幕大小为n行
+/pattern 在每个档案显示前搜寻该字串（pattern），然后从该字串前两行之后开始显示
-c       从顶部清屏，然后显示
-d       提示“Press space to continue，’q’ to quit（按空格键继续，按q键退出）”，禁用响铃功能
-l        忽略Ctrl+l（换页）字符
-p       通过清除窗口而不是滚屏来对文件进行换页，与-c选项相似
-s       把连续的多个空行显示为一行
-u       把文件内容中的下画线去掉
```

##### 4．常用操作命令：

```
Enter    向下n行，需要定义。默认为1行
Ctrl+F   向下滚动一屏
空格键  向下滚动一屏
b  返回上一屏
=       输出当前行的行号
：f     输出文件名和当前行的行号
v      调用vi编辑器
!命令   调用Shell，并执行命令 
q       退出more
```

#### Linux命令head与tail
head 与 tail 就像它的名字一样的浅显易懂，它是用来显示开头或结尾某个数量的文字区块，head 用来显示档案的开头至标准输出中，而 tail 想当然尔就是看档案的结尾。 
##### 1．命令格式：
    head [参数]... [文件]...
###### 2．命令功能：
head 用来显示档案的开头至标准输出中，默认head命令打印其相应文件的开头10行。 
##### 3．命令参数：
```
-q 隐藏文件名
-v 显示文件名
-c<字节> 显示字节数
-n<行数> 显示的行数
tail -f filename 可以不断刷新显示
```

#### linux下cat命令详解

cat主要有三大功能： 
 1. 一次显示整个文件。`$ cat filename`
 2. 从键盘创建一个文件。`$ cat > filename` . (只能创建新文件,不能编辑已有文件)
 3. 将几个文件合并为一个文件： `$cat file1 file2 > file`

##### 命令参数： 
```
-n 或 --number 由 1 开始对所有输出的行数编号 
-b 或 --number-nonblank 和 -n 相似，只不过对于空白行不编号 
-s 或 --squeeze-blank 当遇到有连续两行以上的空白行，就代换为一行的空白行
-v 或 --show-nonprinting 
```
##### 举例 
把 textfile1 的档案内容加上行号后输入 textfile2 这个档案里 
```
cat -n textfile1 > textfile2
```
这条命令会直接清空textfile2

把 textfile1 和 textfile2 的档案内容加上行号（空白行不加）之后将内容附加到 textfile3 里。 
```
cat -b textfile1 textfile2 >> textfile3
```
这条命令不会清空textfile3,而是追加在textfile3后.

#### linux终端使用快捷键
命令行光标移动：
`crtl+a` 移动到命令行首
`crtl+e` 移动到命令行尾
`crtl+u` 从当前光标所在位置向前清除命令

***
(EOF)

