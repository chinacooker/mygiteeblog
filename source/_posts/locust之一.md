---
title: locust之一
date: 2020-01-15 16:29:40
categories:
	- locust
tags:
	- locust
	- 性能测试
---
## 背景
由于对java不是很熟悉，对python有一定的了解，最近对于性能又有了解的需求，之前一直是用jmeter做的性能测试，搜了下，python也有locust的，于是乎，开干。
## 环境
python3.6.8,Flask==0.12
mac10.14.6
## 安装
```
pip  install  locust
```
安装完
```
locust  --version
```
<!-- more -->
安装过程遇到些坑，遇到了网上找找教程哈哈。<br>
查看后版本如下：
locust 1.1.1(版本很重要啊，网上的资料很多都是之前的版本，很多已经在新的版本上已经不好用了)
## 基础
直接上代码：
```
from locust import HttpUser, task,between
class ScriptTasks(HttpUser):
    wait_time = between(5, 9)
    def on_start(self):
         self.client.post("/login", {"username": "test","password": "123456"})
    @task(1)
    def index(self):
        self.client.get("/")
    @task(2)
    def about(self):
        self.client.get("/about/")
    @task(3)
    def demo(self):
        payload={}
        headers={}
        self.client.post("/demo/",data=payload, headers=headers)
```
依次解读：
wait_time = between(5, 9)表示每个用户的等待时间<br>
on_start方法类似于初始化，不在task里面<br>
task解释器表示一个个的任务，后面的括号表示执行的权重，这个case里index，about和demo的权重是1：2：3<br>
不难发现，我们没有定义用户数，甚至域名也没有定义。好了，不买关子了，下一步我们继续。
## 开始测试
```
locust   -f  filepath/*.py
```
-f直接指定测试文件<br>
这样会在本地8089端口启动一个服务，可以在本地
```http://localhost:8089/```开启ui页面
![](/images/locust-config.png)
可分别设置：<br>
```Number of total users to simulate```<br>
```Hatch rate (users spawned/second)```<br>
```Host (e.g. http://www.example.com)```<br>
然后点击```Start swarming```开始测试就可以了<br>
开始后我们就可以看到执行的各个方法的各项统计了，也会有简单的性能图。
![](/images/locust-statistics.png)
![](/images/locust-charts.png)
tips:如果我们将测试文件命名成locustfile.py，在当前目录则直接用locust命令会默认执行这个文件。



