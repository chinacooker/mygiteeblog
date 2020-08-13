---
title: git简单教程二
date: 2020-01-05 13:59:45
categories:
	- 版本控制
tags:
	- git
---
## 初始化仓库，修改，提交
上一篇我们说了有远端代码基础上，我们clone一个基础的版本下来，然后在这个基础上进行工作，如果我们直接不管远程，直接本地开干呢？
首先，我们随便建一个文件，这个就作为我们的工作目录了,(添加一些内容到这个文件)，然后开始生成一个仓库（本地库）：
```
$git init
初始化空的 Git 仓库于 /../.git/
```
我们会发现这个文件夹下多了个.git的隐藏文件夹，这个是文件夹就是Git来跟踪管理版本库的，上一篇我们clone下来的仓库是会默认带有这个信息的(千万不要动这个文件夹)。
好了，这个第一步git init，但是这一步只有在第一次的时候需要这么做，称为初始化。
<!-- more -->
初始化后假如有未提交的内容，我们用status可以查看到状态：
```
$git status
位于分支 master

初始提交

未跟踪的文件:
  （使用 "git add <文件>..." 以包含要提交的内容）

	a.md
	b.md
	c.md

提交为空，但是存在尚未跟踪的文件（使用 "git add" 建立跟踪
```
提示还是比较友好的，第二步git add + 文件名或者git add .将需要放到仓库的文件添加到仓库，可以指定文件或者全部文件。
例如上面的例子：
```
$git add a.md
```
我们add后再次查看状态：
```
$git status
位于分支 master

初始提交

要提交的变更：
  （使用 "git rm --cached <文件>..." 以取消暂存）

	新文件：   a.md

未跟踪的文件:
  （使用 "git add <文件>..." 以包含要提交的内容）

	b.md
	c.md
```
第三步就是把添加的文件提交到仓库，用commit：
```
$git commit -m "test"
[master （根提交） d56dc4a] test
 1 file changed, 1 insertion(+)
 create mode 100644 a.md
```
这样我们就完成了一次提交了。
如果这个时候我们还有2个文件没提交，我们切到其他分支会怎样呢？
切换分支后，status如下：
```
$git status
位于分支 dev
未跟踪的文件:
  （使用 "git add <文件>..." 以包含要提交的内容）

	b.md
	c.md

提交为空，但是存在尚未跟踪的文件（使用 "git add" 建立跟踪）
```
可知没有添加到仓库的文件其实是可以提交到任何分支的，相当于还在暂存区，跟仓库还没什么关系，我们理论上是可以一次工作提交不同文件到不同分支的。
但是，开始使用还是建议git add .添加所有的改动吧。
## 修改了什么
假如我们修改了内容，还没有add和commit，我们用git status查看的时候应该可以知道某个文件被修改了，但是还没放到仓库。
可以git diff下：
```
$ git status
位于分支 master
尚未暂存以备提交的变更：
  （使用 "git add <文件>..." 更新要提交的内容）
  （使用 "git checkout -- <文件>..." 丢弃工作区的改动）

	修改：     a.md

修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）
$ git diff a.md
diff --git a/a.md b/a.md
index 58c9bdf..69840a4 100644
--- a/a.md
+++ b/a.md
@@ -1 +1,4 @@
-111
+1112
+sss
+321
+

```
这样的话，我们就算没提交到工作区，溜达一年回来也没事了。
## 提交记录，版本回退
我们要看提交的记录怎么办，直接用git log：
```
$git log
commit a079514d1a411db612e6a4eaa4e8041101c8e1fa
Author: xxx
Date:   Tue May 19 15:40:54 2020 +0800

    test2

commit d56dc4a11a197a8afe012d9fe2932613a5b5ca92
Author: xxx
Date:   Tue May 19 14:50:20 2020 +0800

    test

```
我们用git show可查看最近的一次commit，+commitid可查看指定的commit，也可以commitid后面再加filename查看那次提交的某个文件的修改；你应该可以看到备注，这个就是我们commit的时候-m后面跟的备注，所以写好备注吧，对于你管理版本非常重要。<br>
或许我们会问了,看到log怎么了，仅仅是看看？
既然有记录，那我们可以回去啊,git reset可以回到最近的版本：
```
$git reset --hard HEAD^
HEAD 现在位于 d56dc4a test
```
当然也可以用git reset --hard+commitid回到指定的commit版本。
我们回去了，新版的代码没了，可是我想反悔了怎么办啊？
```
$git reflog
d56dc4a HEAD@{0}: reset: moving to d56dc4a11a197a8afe012d9fe2932613a5b5ca92
ea3d733 HEAD@{1}: commit: test1
d56dc4a HEAD@{2}: reset: moving to HEAD^
a079514 HEAD@{3}: commit: test2
d56dc4a HEAD@{4}: checkout: moving from dev to master
5995928 HEAD@{5}: commit: testdev
d56dc4a HEAD@{6}: checkout: moving from master to dev
d56dc4a HEAD@{7}: commit (initial): test
```
我们找到了历史的操作记录，前面就是commitid，直接用这个短id配合git reset就可以回到指定的版本了。
## 取消修改和取消暂存
上面是commit为维度的回退，如果还没有commit，如果还没有add呢？
这个简单，一种办法是直接改文件改回来就是了，还有一种办法，用git checkout -- filename,注意，--后面有个空格，成功后不会有任何的提示的哟，但是我们通过git status应该可以发现暂存区的修改没了。
那如果add了，但是没有commit呢？
用git reset HEAD+filename就好了。其实这些信息在git status时都是可以看到有命令提示的：
```
$git status
位于分支 master
要提交的变更：
  （使用 "git reset HEAD <文件>..." 以取消暂存）

	修改：     a.md

尚未暂存以备提交的变更：
  （使用 "git add <文件>..." 更新要提交的内容）
  （使用 "git checkout -- <文件>..." 丢弃工作区的改动）

	修改：     a.md
```
总之就是git checkout -- filename取消未暂存的修改，git reset HEAD+filename取消已暂存未提交的修改。
## 删除仓库文件
本地删除一个文件后，要从仓库中也删掉，需要用git rm +filename来操作。