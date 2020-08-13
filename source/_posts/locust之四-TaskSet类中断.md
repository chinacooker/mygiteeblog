---
title: locust之四-TaskSet类中断
date: 2020-01-25 11:33:38
categories:
	- 性能测试
tags:
	- locust
	- python
---
## 背景
TaskSet类进入后，类本身是不会主动退出这个类的，如果要退出这个类需要怎么做的，答案是需要TaskSet.interrupt（）方法来完成。
## TaskSet.interrupt
伪代码如下:
```
class TaskSection(TaskSet):
    @task(5)
    def about(self):
        self.client.get("/about/")
    @task(5)
    def demo(self):
        payload = {}
        headers = {}
        self.client.post("/demo1/", data=payload, headers=headers)
    @task(1)
    def stop(self):
        self.interrupt()
class ScriptTasks(HttpUser):
    tasks = {TaskSection:1,demo2:3}
    wait_time = constant(1)
```
利用这个功能，结合任务的权重比例，理论上我们可以模拟某组动作的结束的可能性（如退出系统等）
