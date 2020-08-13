---
title: shell基础之一
date: 2020-02-05 14:40:34
categories:
	- shell
tags:
	- linux
---
## 基础
### echo
```
echo xx
echo 'xx'
echo "xx"
```
上面的几种等效。<br>
如遇到特殊符号，可加反斜杠‘\'，如下：
```
echo "xx \!"
```
其实打印是就是“xx ！”
### printf
基本功能和上述的echo一致，区别就是printf打印默认不换行，我们来看下更高级的功能。<br>
```
printf  "%-5s %-10s %-4s\n" No Name  Mark
```
其实就是参数化<br>
%-5s 左对齐补齐5位（+则是右对齐）;  
当然还有%-4.2f 左对齐，补足4位，2位小数，不足补0（有其他语言基础的都不难理解）;  
\n 换行
<!-- more -->
### 文本颜色和背景
``\e[1;31m xxx  \e[0m`` 
颜色31m，指红色字体，如果是41m则是绿色背景。  
关于31m，41m等转义序列，可通过``man console_codes``查看
```
man console_codes  | grep  background  #背景
man console_codes  | grep  foreground  #字体
```
### 变量
```
var="value" 
echo $var  #value
echo "$var x"  #value x
echo ""$var"x x" #valuex x
```
echo，printf可以引用变量，引用使用$var，可用“”与其他元素隔开。
### 数学运算和高级运算
```
let var=1+1
echo $var #2
echo "4*0.56"|bc #2.24
echo "scale=2;22/7" | bc #3.14,设置小数精度
echo "obase=2;100" | bc #10进制转2进制
```
### 重定向
```
echo "xx">1.txt #覆盖文件
echo "yy">>1.txt #在文件底部追加
```
上面是标准输出重定向stdin，如果是错误输出的话，需要如下操作：
```
ls+ 2>2.txt 
```
也可以一次定义好标准输出和错误输出：
```
cmd 2>3.txt 1>4.txt #如果错误输出会到3.txt，标准输出回到4.txt
```
也可以部分重定向，如果3个文件，其中一个文件无法读取，文件名a1，a2，a3，则``cat a* 2>5.txt``时，
会正常输出a2和a3有权限的文件信息，5.txt会记录错误信息。
还有个命令tee只能读取stdout数据，也可玩一些花样，不做赘述了。
### 输入重定向
一方面’<'可从右侧取输入；  
另外还有一些值得值得的骚操作，如下：
```
cat<<EOF>log.txt
```
出现‘>'后，输入任何内容，然后以“EOF”行结束，那输入的内容会在文件首显示，即出现在cat <<EOF>log.txt与下一个EOF行之间的所有文本行都会被当作stdin数据。
### 自定义文件描述符
exec可以任意自定义一个文件描述符，但一般是一次性使用的，不做赘述了。

