---
title: git简单教程三
date: 2020-01-06 17:09:53
categories:
	- dev
tags:
	- git
	- checkout
	- stash
	- pop
	- branch
	- commit
	- cherry-pick
---
## 新建分支
创建分支之前我们说过，就是git checkout -b 分支名，可用（git branch 分支名+git checkout 分支名代替） 。
为了不和前面撤销本地修改git checkout -- <file>混淆，也可以用switch -c来代替，切换到已有分支就跟checkout一致了。
## 合并分支
确保我们的分支在新建的分支，完成修改commit后，再切回想要最终合并的分支如master，则再master分支git merge就可以了。
git merge 分支名，表示把分支合并到master。例如我再test分支新建了个文件，commit后回到master，merge信息如下：
```
$git merge test
更新 01988ae..7a91724
Fast-forward
 c.md | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 c.md
```
注意到Fast-forward，属于快速模式，直接把master分支指向dev的当前提交，其他方式我们后面再看。
<!-- more -->
## 删除分支
合并完了，如果我们想删除分支（实际上开发过程中会建很多分支，一直留着真的影响视觉和效率的），git branch -d 分支名即可。
如果遇到不需要的合并的分支想要删除可以用-D来强制删除。
## 冲突
设想几个情况，文件a，你再master分支修改了第一行并且commit了，切到test分支，也修改了第一行提交了；回到master分支merge test分支时，会保留那个版本呢？
试试呗：
```
$git merge test
自动合并 a.md
冲突（内容）：合并冲突于 a.md
自动合并失败，修正冲突然后提交修正的结果
```
可以看下状态：
```
$git status
位于分支 master
您有尚未合并的路径。
  （解决冲突并运行 "git commit"）

未合并的路径：
  （使用 "git add <文件>..." 标记解决方案）

	双方修改：   a.md

修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）
```
直接看内容：
```
<<<<<<< HEAD
1221111ssad
=======
1111331ssad
>>>>>>> test
ssa11as
saaseads
111
```
比较明显可以看到2个分支的不同，修改自己最终需要保留的版本，重新commit即可。
## 储藏版本，部分合并
如果我们某次修改不想提交也不想取消，可以用git stash。
通过git stash list可以看到储藏的版本，通过git stash pop来恢复现场并且删除储藏的内容，也可以用git stash apply stashid（list的时候可以看到）来恢复指定的stash。
其实上面的操作一般是修bug的时候用的，那修完bug，之前储藏内容的那个版本也是有bug的吧，直接用git cherry-pick+commitid即可，会在工作分支新增一个commit。