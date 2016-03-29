title: "一个较好的Git分支模型"
date: 2015-05-03 10:09:50
tags:
  - Git
categories:
  - 原创
  - 提高效率
thumbnailImagePosition: left
thumbnailImage:http://7xrt06.com1.z0.glb.clouddn.com/16-3-29/42431250.jpg
---

Vincent Driessen 提出的一个比较好的Git分支模型
<!-- more -->
<!-- toc -->

## 一个较好的Git分支模型
[Vincent Driessen](http://nvie.com/posts/a-successful-git-branching-model/)基于自己项目开发的经验提出了一个比较好的Git分支模型：

![gitflow branch](https://ihower.tw/blog/wp-content/uploads/2011/02/Screen-shot-2009-12-24-at-11.32.03.png)

在这个模型中定义了**主要分支**和**辅助分支**两类分支。其中主要分支包括`master分支`和`develop分支`，这两个分支在项目的整个生命周期中存在；辅助分支包括`feature分支`、`release分支`和`hotfix分支`，主要是为了解决一些特定的问题而建立的分支，这些分支随着问题的解决而销毁。

### 主要分支

主要分支是项目的**核心分支**，开发中产生的所有代码最终都会合并到主要分支。主要分支中应该保存**随时可以运行使用的代码**，主要分支包括`master分支`和`develop分支`。在初始时，`master分支`和`develop分支`相同，此时开始基于`develop分支`进行开发，当相应版本的所有功能开发完毕，即可合并到`master分支`形成一个版本。

![main branch](http://nvie.com/img/main-branches@2x.png)

#### master 分支
`master分支`上存放的应该是随时可供在生产环境中部署发布的代码（Production Ready state）。当某一版本的所有功能全部开发完毕，并且测试通过，产生了一份新的可供部署的代码时，`master分支`上的代码会被更新。同时，每一次更新，都会添加对应的版本号标签（TAG）。

#### develop 分支
`develop分支`是保存当前最新开发成果的分支。当某个功能在`feature分支`（下文有介绍）开发完成并且测试通过则会合并到`develop分支`。原则上`develop分支`上的代码也是可以直接拿来运行使用的。

### 辅助分支

辅助分支是用于组织**解决特定问题**的各种软件开发活动的分支。辅助分支主要用于组织软件**新功能的并行开发**、简化新功能开发代码的跟踪、辅助**完成版本发布**工作以及对生产代码的缺陷**进行紧急修复**工作。这些分支与主分支不同，通常存在于有限的生命周期内。

辅助分支有：
 1. 用于新功能开发的`feature分支`
 2. 用于辅助版本发布的`release分支`
 3. 用于进行紧急修复的`hotfix分支`
 
#### feature 分支
`feature分支`主要用于新功能的开发，基于`develop分支`创建。当功能开发完成并且测试通过，`feature分支`要合并回`develop分支`然后删除。`feature分支`的命名可以使用除 `master`，`develop`，`release-*`，`hotfix-*`之外的任何名称，最好使用相应的功能命名，例如需要添加登陆功能，可以命名为`login`。

如果某个功能的开发由单个工程师完成，则`feature分支`代码可以保存在开发者自己的代码库中而不强制提交到远程代码库里，如果需要多人开发某一功能则`feature分支`需要push到远端仓库。

![feature branch](http://www.ituring.com.cn/download/01YiLAQlsJXe)

#### release 分支
`release分支`主要用于版本的发布，当某一版本的所有功能开发完成并且测试通过，需要使用`release分支`进行发布，`release分支`基于`develop分支`创建。在这个分支上的代码允许做小的缺陷修正、准备发布版本所需的各项说明信息（版本号、发布时间、编译时间等等）。最后`release分支`会合并回`master分支`和`develop分支`，并且打上tag然后删除。

当`develop分支`上的代码已经包含了所有即将发布的版本中所计划包含的软件功能，并且已通过所有测试时，我们就可以考虑准备创建`release分支`了。而所有在当前即将发布的版本之外的业务需求一定要确保不能混到`release分支`之内（避免由此引入一些不可控的系统缺陷）。

在实际的开发中`release分支`生命周期可能要很长，因为涉及到bug的修复。

#### hotfix 分支
`hotfix分支`主要用于紧急bug的修复。在运行环境中的版本有可能会出现bug，需要紧急修复，这时候用到`hotfix分支`。`hotfix分`支由`master分支`创建，当bug修复完毕，`hotfix分支`需要合并回`develop分支`和`master分支`，并且打上tag然后删除。

![hotfix分支](http://www.ituring.com.cn/download/01YiLAROllzF)

## Git Flow 

#### Git Flow 是什么
[Vincent Driessen](http://nvie.com/posts/a-successful-git-branching-model/)提出上面模型的同时，提供了一套git命令集合，通过这套命令集合可以很方便的维护项目的开发按照上面的模型进行，这就是Git Flow。

#### Git Flow 安装

```
$ curl -OL https://raw.github.com/nvie/gitflow/develop/contrib/gitflow-installer.sh
$ chmod +x gitflow-installer.sh
$ sudo ./gitflow-installer.sh
```

更多的安装方式参见[Wiki](https://github.com/nvie/gitflow/wiki/Installation)

#### Git Flow 命令

![git flow commands](http://danielkummer.github.io/git-flow-cheatsheet/img/git-flow-commands.png)

#### git flow init
这个命令用来做一些初始化的操作，设定相应分支的名称，推荐使用默认的就好。

如果在一个已经存在的git repo中执行`git flow init`，它会列出所有已经存在的branch让你选择，如下：

```
king@king:~/gitflow$ git flow init 

Which branch should be used for bringing forth production releases?
   - dev
   - master

Branch name for production releases: [master] 

Which branch should be used for integration of the "next release"?
   - dev

Branch name for "next release" development: [develop] 

How to name your supporting branch prefixes?
Feature branches? [feature/] 
Release branches? [release/] 
Hotfix branches? [hotfix/] 
Support branches? [support/] 
Version tag prefix? [] 
```

这里既可以选择用已经有的`dev分支`也可以用默认的`develop分支`，需要注意的是默认的`develop分支`是基于`master分支`创建的。

#### Feature

`Feature 分支` 的创建基于develop，通过下面的命令创建：

```
king@king:~/gitflow$ git flow feature start newFunc
Switched to a new branch 'feature/newFunc'

Summary of actions:
- A new branch 'feature/newFunc' was created, based on 'develop'
- You are now on branch 'feature/newFunc'

Now, start committing on your feature. When done, use:

     git flow feature finish newFunc

king@king:~/gitflow$ git branch 
  dev
  develop
* feature/newFunc
  master

```

这个操作创建了新的分支并且切换到新的分支上。如果这个功能多个工程师共同开发，那么需要推送到远程仓库，执行下面的命令：

```
king@king:~/gitflow$ git flow feature publish newFunc
Total 0 (delta 0), reused 0 (delta 0)
remote: Updating references: 100% (1/1)
To ssh://king@192.168.99.213:29418/~king/gitflow.git
 * [new branch]      feature/newFunc -> feature/newFunc
Already on 'feature/newFunc'

Summary of actions:
- A new remote branch 'feature/newFunc' was created
- The local branch 'feature/newFunc' was configured to track the remote branch
- You are now on branch 'feature/newFunc'
```

如果只有一个开发者，那么不需要推送到远程分支，`feature分支`只需要在开发者本地就行。

现在你可以进行新的特性的开发，当所有功能开发完毕，可以执行下面的命令结束：

```
king@king:~/gitflow$ git flow feature finish newFunc
Switched to branch 'develop'
Updating aba6e76..1d98a66
Fast-forward
 0 files changed
 create mode 100644 newfile
Deleted branch feature/newFunc (was 1d98a66).

Summary of actions:
- The feature branch 'feature/newFunc' was merged into 'develop'
- Feature branch 'feature/newFunc' has been removed
- You are now on branch 'develop'

```

这样新特性就开发完毕了，通过执行上面的命令`feature/newFunc`分支被合并到`develop分支`，然后被删除。这时候你可能需要执行`git push origin develop`命令将`develop分支`推送到远程仓库。

#### Release / Hotfix

这里两个命令和上面`feature`命令使用相同，不过执行的操作略有不同，参见下面的参考资料。另外这两个命令在执行`finish`操作时都会打上相应的tag。你可能需要执行`git push origin tagName`将tag推送到远程仓库。

## 参考资料

[A successful Git branching model](http://nvie.com/posts/a-successful-git-branching-model/)
[git-flow 备忘清单](http://danielkummer.github.io/git-flow-cheatsheet/index.zh_CN.html)
[基于git的源代码管理模型](http://www.ituring.com.cn/article/56870)
[Git flow 開發流程](https://ihower.tw/blog/archives/5140)
[团队使用Git和Git-Flow手记](http://www.toobug.net/article/git_and_gitflow.html)
[git-flow 源码](https://github.com/nvie/gitflow)

----------
（EOF）

