---
title: locust之三-任务分配
date: 2020-01-21 11:33:38
categories:
	- 性能测试
tags:
	- locust
	- python
---
## 任务分配
在jmeter里我们可以执行多个接口的压测，例如测试接口A和接口B，压测的时候又要求A和B的请求比例是1：2，locust也是可以实现的，之前就已经说过了。
## task
伪代码如下，表示task1和2的执行比例是1：2
```
@task(1)
def task1():
	pass
@task(2)
def task2():
	pass
```
<!-- more -->
## user
我们还可以定义多个用户组，在选用户的时候就做比例控制：
```
class user1(HttpUser):
	weight=1
	@task(1)
	def user1task1():
		pass
	@task(2)
	def user1task2():
class user2(HttpUser):
	weight=2
	@task(1)
	def user2task1():
		pass
	@task(2)
	def user2task2():
```
weight是控制user组比例的，如上面的伪代码，user1：user2=1：2，<br>
而user1task1：user1task2：user2task1：user2task2=1：2：2：4
## 不用task装饰器
伪代码如下：
```
class TaskUser(HttpUser):
    tasks = [about,demo]
```
这时task about和demo就无需task装饰器了，HttpUser的父类提供了tasks来选择任务。
我们还可以选择任务的时候加上权重
```
tasks = {about:2,demo:1}
```
上面的这种方式其实类似于
```
tasks = [about,about,demo]
```
## tag装饰器
我们还可以给每个任务定义tags，用法很简单，伪代码如下：
```
from locust import HttpUser, task,between,tag
class ScriptTasks(HttpUser):
    wait_time = between(5, 9)
	@tag('tag1','tag2')
	    @task(1)
	    def index(self):
	        self.client.get("/")
	    @tag('tag1')
	    @task(2)
	    def about(self):
	        self.client.get("/about/")
	    @tag('tag3')
	    @task(3)
	    def demo(self):
	        payload={}
	        headers={}
	        self.client.post("/demo/",data=payload, headers=headers)
```
执行的时候加上``--tag tag1``即可筛选tag为tag1的所有任务，
而``--exclude-tags``可用用来反选。<br>
”Exclusion always wins over inclusion“（排除总是胜于包容），虽然不怎么回去这么做，但如果你需要正选又要反选的时候，那一定是反选的标签即使被正选，那也不会被执行的。
## TaskSet
除了上面的方法，我们还可以用TaskSet类来组织任务，然后还是用tasks来筛选TaskSet类。伪代码如下：
```
from locust import TaskSet, HttpUser,task, constant
class TaskSection(TaskSet):
    @task(1)
    def about(self):
        self.client.get("/about/")
    @task(3)
    def demo(self):
        payload = {}
        headers = {}
        self.client.post("/demo1/", data=payload, headers=headers)
class ScriptTasks(HttpUser):
    tasks = [TaskSection]
    wait_time = constant(1)
```
也可以使用@task装饰器直接在HttpUser类下内联TaskSet：
```
class ScriptTasks(HttpUser):
	@task(1)
    class TaskSection(TaskSet):
```
有个需要注意的点就是：<br>
当运行中的用户线程进入该TaskSet中时，执行完一个任务，等待完会继续执行这个类中的其他任务，而不会重新选择了。至于怎么出来，我相信是有方法的，后续研究先。
## 关于用户执行任务的选择
上一个例子我们可以看到，用户每次不是都重新从一开始的筛选步骤来筛选的。<br>
例如，上面的TaskSet类，用户只要进入某个TaskSet类了，那下一次只限定在这个类里选task；
又例如一个locustfile有不同的User类，其实在启动完所有的User后，就分别限定了所属的User类了，无论执行多少次也只能执行自己User类里的任务了。
