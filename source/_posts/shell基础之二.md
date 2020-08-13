---
title: shell基础之二
date: 2020-02-05 14:40:34
categories:
	- shell
tags:
	- linux
---
## 数组
### 基础
两种定义方式：
```
array1=(x y z)
array2[0]='x'
array2[1]='y'
array2[2]='z'
```
### 关联数组
理解的关联数组有点像字典，例子如下：
```
declare -A fruits_value
fruits_value=([apple]='100 dollars' [orange]='150 dollars')
echo ${fruits_value[apple]}  #100 dollars
```
关联数组和普通数组可以理解为区别在于改了索引，通过下面的方式可以查看：
``echo ${!array[*]}``如果是普通数组，结果会是0 1 2...如果是关联数据则是你重新定义的index值。
<!-- more -->
## 别名alias
知道下即可，需要注意的是，如果是linux系统需要别名永远生效，需要写入~/.bashrc文件，例如：``alias la='ls -a'``,  
在mac则是写入~/.bash_profile,立刻生效还需要source一下。
unalias来取消别名。
## 时间
```
date #2020年 08月 12日 星期三 19:17:45 CST
date +%s  #1597231075,纪元时
date --date "Wed mar 15 08:09:16 EDT 2017" +%s  #1489579756，转化日期
--date +%A #星期
date "+%d %B %Y" #12 八月 2020 （中文还是英文可能和系统的设置有关系，不要关心）
aA工作日；bB月；dD日；yY年；IH小时；M分；S秒；N纳秒；s Unix时间
```
## 调试
```
bash -x  bashfile
```
可以再bash文件里限制调试区域，用``set -x``和``set +x``隔开即可。
+x禁止调试  
-v当命令进行读取时显示输入  
+v禁止打印输入  
下面2个不大懂
### debug
```
#！/bin/bash
function DEBUG()
{
	[ "$_DEBUG" == "on" ] && $@ || :
}

for i in {1..10}
do
	DEBUG echo "I is $i"
done
```
在需要debug的地方前面加上DEBUG，执行文件使用``_DEBUG=on bash bashfile``即可
