---
title: shell基础之四
date: 2020-02-08 14:40:34
categories:
	- shell
tags:
	- linux
---
## 管道
``|``
## 字符分割器
```
oldIFS=$IFS
IFS=,
data="name,gender,rollno,location"
for item in $data;
do
        echo Item: $item
done
IFS=$oldIFS
```
输出：
```
Item: name
Item: gender    
Item: rollno
Item: location
```
处理一些日志的时候可以参考。
<!-- more -->
## 循环
### for
```
for var in list;
do
	commands;
done
```
```
for(()i=0;i<10;i++)) #注意是2个括号
{
	commands;
}
```
```
for i in lists;
do
	commands;
done
```
### while
```
while condition
do
	commands;
done
```
### until
```
x=0
until [ $x -eq 9 ];  #[]前后一定要加空格
do 
let x++;
echo $x;
done
```
## 条件
```
if condition;
then
	commands;
fi
```
```
if condition;
then
	commands;
else if condition;then
	commands;
else
	commands;
fi
```
## 比较
前面其实已经有在用了，用[]来比较：
```
[ $var -eq 0 ]
-eq #等于
-gt #大于
-lt #小于
-ge #大于等于
-le #小于等于
-ne #不等于
-a #逻辑与
-o #逻辑或
-f -x等 #文件相关
```
