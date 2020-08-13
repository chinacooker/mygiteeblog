---
title: shell基础之三
date: 2020-02-08 14:40:34
categories:
	- shell
tags:
	- linux
---
## 函数
以下2种方式定义都可以
```
function f(){ xx;}
f(){ xx;}
```
### 参数
```
f(){
	echo $1;  #打印第一个参数
	echo "$@";  #打印每一个参数
	echo "$*";  #按整体打印所有参数
	echo $0;  #打印函数名称
	return 0;
}
f x  y  #传参x，y
```
其实别名和函数有点类似，只是函数可能可以实现更加复杂的功能而已。
