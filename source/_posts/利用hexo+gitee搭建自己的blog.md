---
title: 利用hexo+gitee搭建自己的blog
date: 2020-01-01 00:00:00
categories:
	- blog
tags:
	- node
	- markdown
	- git
	- blog
	- gitee
---
## 今年，我们需要有自己的博客
### Hexo
Hexo是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。
本地快速部署很简单：
```
npm install hexo-cli -g
hexo init blog
cd blog
npm install
hexo server
```
不难发现，我们需要提前安装node。执行完最后一行代码后，我们的本地已经跑起来了，默认在4000端口。
本地我们自己玩和debug就好，博客还是要给大家看的，所以我们需要一台服务器，碰巧的是hexo兼容了gitlab、github，我们可以用免费的服务，但是由于众所周知的原因，还是建议用国内的服务商gitee。
<!--more-->
### Gitee
注册，绑定手机和邮箱这个就不多说了。
新建一个和用户名一致的仓库，即路径、仓库、归属保持一致，要给大家看肯定要公开，然后创建就可以了。
仓库建完，仓库可选服务-Gitee Pages，都是默认的就好。
服务有了下面就是部署了，我们应该可以在blog目录下找到配置文件_config.yml,有如下两个部分需要修改：
url改成你gitee的地址，如```https://xxx.gitee.io/```
```
deploy:
  type: git
  repository: 
    gitee: https://gitee.com/xxx/xxx.git
```
然后还是在本地的blog目录下，执行下面的代码，执行前确保你本地下载了git，绑定本地git和远程仓库的方法就不说了（配置ssh）：
```
git config --global credential.helper store
git config --global user.name "Your's name"
git config --global user.email "Your's email address"
hexo clean;hexo g;hexo d
```
记住hexo server本地服务最好停一下，部署完之后然码云的服务页面更新下部署
### 写作
上面的```hexo server,hexo clean,hexo g,hexo d```都是常用的命令
其中hexo server又可简写成hexo s，本地部署时当资源发生变化，页面会自动重新加载
hexo，hexo g实际是部署前预生成静态文件的组合操作，hexo d是部署到远程的操作
我们关心的写作很简单hexo new +articlename即可，在本地```source/_post```文件夹下面可以找到你刚建的文章,在里面进行markdown的编辑，然后hexo clean，hexo g，然后本地或者远程部署就行了。





