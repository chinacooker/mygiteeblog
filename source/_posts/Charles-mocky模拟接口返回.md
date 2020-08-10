---
title: Charles+mocky模拟接口返回
date: 2020-01-10 17:13:05
categories:
	- mock
tags:
	- mock
	- mocky
	- charles
	- proxy
---
## 环境
mac 10.14.6
charles 4.5.6
## 操作步骤
- 使用charles抓本地网络请求包：Proxy->MacOS-Proxy 
- 找到你要mock的请求
- 右键选择Map Remote...
- Map To输入Protocol，Host和Path
- 确保Tools->Map Remote生效
- Protocol，Host和Path的信息由mock产出

<!-- more -->

## mock
打开[https://www.mocky.io/](https://www.mocky.io/)
选择对应的响应码和数据格式，填上需要mock的数据即可（一般是按自己的需要造，或者已有返回数据来修改）
## 应用场景
- 前端开发调试已有后端接口返回格式，但是还没后端接口没数据或者后端接口数据不全
- 前端逻辑依赖后端的返回数据比较复杂和多样，测试可以借助该方法先进行前端功能的验证
