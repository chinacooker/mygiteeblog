---
title: locust之二-等待时间
date: 2020-01-18 09:32:25
categories:
	- 性能测试
tags:
	- locust
	- python
---
## 等待时间
等待时间是指每个用户执行完一次操作后的等待时间，一般有3种形式：
between，constant，constant_pacing,使用的时候直接import即可
```
wait_time = between(5, 15)  #单位秒,时间范围
```
```
wait_time = constant(1)  #指定时间1s
```
```
wait_time = constant_pacing(1)  #指定时间
```
其中constant是上一次请求结束后的等待时间，与between定义的一致，而constant_pacing定义的是从上一次请求开始算的等待时间，即如果设置了1s，而上一次请求就花了超过1s，下一次请求就无需等待了。
## 等待时间方法
<!-- more -->
也可以定义一个等待时间的方法，伪代码如下：
```
last_wait_time = 0
    def wait_time(self):
        self.last_wait_time += 1
        return self.last_wait_time
```
函数wait_time效果跟wait_time一致，只是相当于重写了这个方法。