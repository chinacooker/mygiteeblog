---
title: locust之五-进阶
date: 2020-01-29 03:32:25
categories: 
	- locust
tags:
	- locust
	- 性能测试
	- FastHttpUser
	- headless
	- step-load
	- step-users
	- conf
---
## FastHttpUser
我们一般默认都是使用HttpUser这个类，但如果我们需要更好的http请求性能，可以使用FastHttpUser，但是FastHttpUser并不能兼容所有的HttpUser的功能，特别是post请求，大家可以自己尝试下。<br>
使用FastHttpUser也很简单，import后直接将HttpUser改为FastHttpUser就可以了。
```
from   locust.contrib.fasthttp import   FastHttpUser
```
## headless
locust也有无头模式，如下：
```
locust -f tasks.py  --headless -u 1000 -r 100 --run-time 30m --step-load --step-users 300 --step-time 1m   --host=http://www.baidu.com
```
<!-- more -->
-headless表示无头
-u表示模拟用户数
-r表示每秒产生的用户数
--run-time表示运行时间
--step-load --step-users --step-time 则是步进模式，表示步进的用户数和每步的持续时间
--host则是测试域名
## config
每次写配置太麻烦了？locust还支持将配置参数写到conf文件，xx.conf文件即可。
伪代码如下：
```
locustfile = tasks.py
headless = true
host = http://www.baidu.com
users = 4000
hatch-rate = 4000
run-time = 1m
```