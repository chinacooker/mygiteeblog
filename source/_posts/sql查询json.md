---
title: sql查询json
date: 2020-07-01 14:40:34
categories:
	- sql
tags:
	- sql
	- mysql
	- postgres
	- pg
	- json
---
现在的数据结构有越来越多的json格式，今天分享一下sql语句查询json的方法，主流的数据库mysql和postgres分别如下，假如表tb_A中有这样一个字段name,数据格式是:
```
{
	"config":"xxx"
}
```
## postgres
name::json->>'config'
## mysql
SON_EXTRACT(name,'$.config')

