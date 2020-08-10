---
title: pytest之二
date: 2020-01-12 11:29:40
categories:
	- pytest
tags:
	- pytest
	- python
	- unittest
---
## pytest执行覆盖
文件：test_开头或者_test结尾；<br>
方法：test开头
类：Test开头，不带__init__方法
类方法：符合要求的类下面符合要求的方法
## 一些常用参数
-x 失败停止<br>
--maxfail=1 失败次数停止<br>
-k 按名称执行<br>
-m 安mark的名字  @python.mark.name<br>
-v 详细信息<br>
-s 不打印print<br>
-q 静默模式<br>
<!-- more -->
pytest --showlocals # 在追溯信息中显示局部变量   <br>
pytest -l           # 显示局部变量 (简写)<br>
pytest --tb=auto    # (默认) 第1和最后1条使用详细追溯信息，其他使用简短追溯信息<br>
pytest --tb=long    # 详尽，信息丰富的追溯信息格式<br>
pytest --tb=short   # 简短的追溯信息格式  <br>
pytest --tb=line    # 每个失败信息一行<br>
pytest --tb=native  # Python标准库格式  <br>
pytest --tb=no      # 不使用追溯信息   <br>
pytest --durations=10 #耗时最长的case<br>
## 调试模式
--pdb调试模式
import sys;sys.last_traceback.tb_lineno;sys.last_value<br>
sys.last_traceback;sys.last_type;sys.last_value<br>
--trace直接进调试<br>
代码设置断点import pdb; pdb.set_trace()<br>
n+enter执行下一步<br>