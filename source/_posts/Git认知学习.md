---
title: Git认知学习
date: 2017-08-20 20:30
tags: [技术,Git]
---
### 前言
*** 
Git教程：[廖雪峰的Git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
今日根据此教程认知和操作了Git，对于Git的概念理解和命令操作有了一定的熟练度。

### 教程
*** 
* 创建仓库,把当前目录变成Git可以管理的仓库：
> git init

* 文件添加到仓库,需要两步：
> git add readme.txt    //添加到暂存区，可反复多次使用，添加多个文件
	git commit -m "wrote a readme file"//暂存区的所有内容提交到当前分支

* 查看工作区状态
> git status//告诉你是否有文件被修改过
> git diff //查看修改内容

* 回退

```
HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，
使用命令git reset --hard commit_id。

穿梭前，用git log可以查看提交历史，以便确定要回退到哪个版本。
要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本
```
* 管理修改

```
Git是如何跟踪修改的，每次修改，如果不add到暂存区，那就不会加入到commit中
```
* 撤销修改

```
场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，
	  用命令：git checkout -- file。
场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修
	  改，分两步，
	  第一步用命令git reset HEAD file，就回到了场景1，
	  第二步按场景1操作。
场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退	
	  一节，不过前提是没有推送到远程库。
```
* 本地库关联远程库
> git remote add origin git@server-name:path/repo-name.git；

* 关联后，推送文件到远程库
> git push -u origin master    //第一次推送
> git push origin master    //以后直接

```
关联推送，可能报错：
报错:Permission denied (publickey).
	fatal: Could not read from remote repository.
解决办法：删除远端和当前key，重新生成key， 
	ssh-keygen -t rsa -C 564239493@qq.com连敲两次回车键
	会在本地C:\Users\你的用户名.ssh生成文件夹，里面有id_rsa和 
	id_rsa.pub两个文件 
	然后复制id_rsa.pub文件里面的内容，到
	https://github.com/settings/keys新建一个，
	然后重新push即可
```
* 先建远程库，本地clone

```
要克隆一个仓库，首先必须知道仓库的地址，然后使用git clone命令克隆。
Git支持多种协议，包括https，但通过ssh支持的原生git协议速度最快。
```
* **分支**
> Git鼓励大量使用分支：
	查看分支：git branch
	创建分支：git branch <name>
	切换分支：git checkout <name>
	创建+切换分支：git checkout -b <name>
	合并某分支到当前分支：git merge <name>
	删除分支：git branch -d <name>

* 解决冲突

```
当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。
用git log --graph 命令可以看到分支合并图。
```
* **分支策略**

```
Git分支十分强大，在团队开发中应该充分应用。

合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支， 
能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。

master用于正式发布版本，dev分支用户干活，每个人有自己的分支user1，然后merge到dev上，然后dev再merge到master
```
* bug分支

```
修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；

当手头工作没有完成时，先把工作现场 git stash 一下，然后去修复bug，修复后，再 git stash pop，回到工作现场。
```

### 后记
*** 
本文仅根据今日学习的笔记做个博客，详细学习还需看 [廖雪峰的Git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000) 。

后续，可能会先再次了解Hexo,然后就是JavaScript，java的基础巩固，穿插着对于前端vue.js,react等的学习。
 