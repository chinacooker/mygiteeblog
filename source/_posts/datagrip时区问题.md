---
title: datagrip时区问题
date: 2020-01-13 09:11:40
categories:
	- sql
tags:
	- datagres
	- sql
	- 时区
	- timezone
	- UTC
	- PRC
---
## 时区显示
datagris配置数据源就不多做介绍了，我们如果发现服务端查数据和客户端查数据不一致时，需要看一下时区是不是正确。
以pg库为例，可以用
```
show timezone
```
来查看时区
## 修改本地客户端时区
如果不一致需要将客户端的默认时区修改一下。<br>
例如客户端是PRC时区，那我们可以修改数据源的默认时区，路径：Properties-Advanced-VM options。
```
-Duser.timezone=PRC
```
