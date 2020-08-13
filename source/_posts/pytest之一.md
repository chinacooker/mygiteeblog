---
title: pytest之一
date: 2020-01-11 09:29:40
categories:
	- 自动化测试
tags:
	- pytest
	- python
---
## 入门
pytest是unnittest的升级版，需要下载pip install pytest即可
test1.py如下：
```
def test_pass():
	assert 1==1
```
直接pyteest test1.py即可
.和F代表通过和不通过，pytest -v可看详细执行信息，-h看更多参数用法
<!-- more -->
捕获异常
test2.py如下
```
import  pytest
import socket
s=socket.socket()
def test_raise():
    with pytest.raises(TypeError) as e:
        s.connect(('localhost','6397'))
    exec_msg = e.value.args[0]
    assert exec_msg == 'an integer is required (got type str)'
```
同样的pytest test2.py即可
全部执行
和unittest一样的是，执行某个文件的时候，test开头的文件都会执行，与之不同的是，以test结尾的也是可以的
例如这样的test3.py：
```
def test_func1():
    assert 1 == 1
def test_func2():
    assert 1 != 1
```
部分执行,以test3.py为例<br>
指定:
只调用某个test方法时这样用pytest test3.py::test_func1;只能指定1个方法<br>
模糊:
pytest -k fun test3.py，只能指定某类名字的，但是需要命名的时候限制的就大了
pytest.mark&&-m参数<br>
上面两种方法多少都有不好的地方，所以可以跟unittest一样用-m参数另外再配合pytest的mark方法<br>
如这样一个mark.py：
```
import pytest
@pytest.mark.finished
def test_func1():
    assert 1 == 1
@pytest.mark.unfinished
def test_func2():
    assert 1 != 1
```
用法很好鉴别@pytest.mark.attribute，调用则是pytest -m attribute mark.py
没执行的会显示：1 deselected,1为未选择的测试数量<br>
一个测试用例多个标记和多个测试用例同样的标记可以自己试一下，-m传参大概是这样的pytest -m “finished and not merged”
<br>
跳过测试:
@pytest.mark.skip(reason=’reason’)
@pytest.mark.skipif(表达式,reason=’reason’)


