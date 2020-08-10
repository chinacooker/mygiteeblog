---
title: git简单教程一
date: 2020-01-02 10:50:56
categories:
	- dev
tags:
	- git
	- github
	- gitlab
	- gitee
	- branch
	- clone
	- push
	- pull
	- status
	- checkout
---
## 安装
安装就不多说了，linux，mac，windows都是支持的，大家网上可以找到很多教程。
## 配置
安装完了后，我们需要配置下你的个人信息，主要是name和email：
```
git config --global user.name "xxx"
git config --global user.email "xx@xxx.xx"
```
不必担心冒用，首先一般没人冒用，其次就算冒用有也办法查的。<br>
不管是github，gitlab，还是gitee（国内服务），使用前我们都要注册账号登录后在设置页面添加我们的ssh公钥，我们把.ssh/id_rsa.pub这个文件的内容粘贴过去即可。
<!-- more -->
如果你本地没有ssh公钥，生成一个吧，方法google或者baidu一下。
## 远程有仓库
我们现在远程仓库建一个仓库，带上readme方便做简单的演示。<br>
cd到你准备放远程代码的目录，git clone + 仓库clone的代码
```
git clone git@github.com:xxx/yyy.git
```
其中xxx是你账号的名称，yyy是你建立的账户的名称，上面是用的ssh的方式clone，一般也建议你用这种方式,除非你们公司没有开放ssh。
如果一切顺利，你会在当前目录看到yyy文件夹，这就是你刚刚clone的远程仓库了，cd进去看看吧。<br>
实际上，git clone克隆的不仅仅是代码信息，还有整个仓库的分支信息，提交信息等，并且会默认将本地的master分支和远程的master分支关联。
```
$git status
位于分支 master
您的分支与上游分支 'origin/master' 一致。
无文件要提交，干净的工作区
```
如果你在远端有多个分支，可以完美的clone多个分支的记录。但是本地只会默认有一个master分支。
可用查看分支：
```
$git branch -a
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/master
  remotes/origin/newbranch
```
如果我们修改本地master分支内容提交后可以直接使用git push推送到远端，而如果使用git pull则会拉下远端的所有的更新。
不管是push或者pull，更新的只是对应的分支，即如果本地master关联的是远程的master，那本地master更新push后只会更新远程的master；同样的，远程master更新后pull也只是更新本地的master。
## 本地新分支工作
平时我们工作在master直接干的操作是比较少的，要不是本地新建的分支，完工后合到本地master再push到远程；要不本地新建个分支，直接pull到远程对应的分支，然后远程的分支合到远程的master。
首先新建一个分支很简单：
```
$git checkout newbranch
分支 newbranch 设置为跟踪来自 origin 的远程分支 newbranch。
切换到一个新分支 'newbranch'
```
我们建了一个分支，并且关联到了远端(注意不能加-b参数)，这个是在远程的分支和新建的分支名称一样的情况下会自动关联。
我这边的git版本是2.25.0，看到一些教程需要用git checkout -b dev origin/newbranch来实现一样的功能。
如果名字不一样，创建完分支后可以用下面的命令来关联：
```
git branch –set-upstream 本地新建分支名 origin/远程分支名
```
新版本用
```
git branch --set-upstream-to=origin/远程分支名  本地新建分支名
```
本地合完再push，以及不关联情况下的直接push就暂时不在这里说了，一般能研究到这里了相信你是可以找到办法的。