---
title: git简单教程终章
date: 2020-01-09 18:15:08
categories:
	- 版本控制
tags:
	- git
---
## 添加远程仓库，关联远程分支
之前除了一开始我们从远程仓库clone下来基础版本工作，一直是本地在操作，但实际的工作中，我们需要将你的东西放到远程仓库，同样你的同事也是，你们来一起构建这个项目。
我们可以用git remote来查看远程库的信息，一般clone下来的项目默认会有一个远程的仓库信息，但是本地的项目是没有的。
我们先来添加一个（远程把仓库先建出来）：
```
git remote add origin git@github.com:xxx/yyy.git
```
xxx是你的github用户名，yyy是你的远程仓库名字，origin是你取的远程仓库的名字，以后你可以用这个名字来推送版本。
<!-- more -->
这个时候我们用git remote -v可以看到远程仓库信息。
```
$git remote -v
origin	git@github.com:xxx/yyy.git (fetch)
origin	git@github.com:xxx/yyy.git (push)
```
使用git push -u origin master即可把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。
```
$git push -u origin  master
对象计数中: 20, 完成.
Delta compression using up to 4 threads.
压缩对象中: 100% (12/12), 完成.
写入对象中: 100% (20/20), 1.52 KiB | 0 bytes/s, 完成.
Total 20 (delta 3), reused 0 (delta 0)
remote: Resolving deltas: 100% (3/3), done.
To git@github.com:chinacooker/addremotedemo.git
 * [new branch]      master -> master
分支 master 设置为跟踪来自 origin 的远程分支 master。
```
其中master是本地分支的意思，假如推送一个本地分支，但是远程没有的，推送的时候会在远程建一个同名的分支出来并关联上。
tips：
- 尽量来一些骚操作吧，本地推送到远端还是同步分支推吧，虽然可以跨着来，但是没必要啊。
- 远程合作的时候如果有冲突的时候回push不成功，需要先pull下来解冲突，而且一般情况下，我们都先pull下。