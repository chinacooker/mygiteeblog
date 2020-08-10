---
title: vue学习之一——起步
date: 2020-02-01 16:48:09
categories:
	- fe
tags:
	- vue
---
## vue是什么
准确的说，vue是vue.js，官网介绍说是“一套用于构建用户界面的渐进式框架”，好吧我承认我并不懂。
我们大概记住几个关键词吧，“自底向上逐层应用”、“只关心图层”，我们就这样带着迷惑开始吧。
## 快速开始
和其他技术一样，我们来个快速起步。
```
<!DOCTYPE html>
<html>
<head>
	<meta charset="utf-8">
	<title></title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>
<body>
<div id="app">
  {{ message }}
</div>
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})
</body>
```
vue的第一个应用起来了，直接在浏览器打开吧。
<!--more-->
## 数据和DOM
其实这时我们的数据已经和DOM关联了，所有的东西都是“响应式”的。<br>
如果我们用的是chrome的话，可以使用插件Vue.js devtools，本地化经常需要在插件管理页面选择允许访问文件地址，不然及时加载了插件报Vue.js not detected的错误。<br>
一切正常的话，当我们打开刚刚的页面的时候，我们会在我们的开发者工具里面看到vue这个选项，可以修改data下面的message值，我们会发现显示会相应地更新。<br>
用官方文字来解释就是：我们不再和 HTML 直接交互了。一个 Vue 应用会将其挂载到一个 DOM 元素上 (对于这个例子是 #app) 然后对其进行完全控制。那个 HTML 是我们的入口，但其余都会发生在新创建的 Vue 实例内部。
tips:vue的el值例如是app-1，我们实际在操作console的时候使用app1来调用即可。
## vue式绑定
除了上面这种简单粗暴的文本绑定外，vue还可以像下面这样来绑定元素：
```
<div id="app-2">
  <span v-bind:title="message">
    鼠标悬停几秒钟查看此处动态绑定的提示信息！
  </span>
</div>
```
```
var app2 = new Vue({
  el: '#app-2',
  data: {
    message: '页面加载于 ' + new Date().toLocaleString()
  }
})
```
v-bind这种在vue里被称为指令，一般是attribute前面带有v-，是vue提供的特殊属性。<br>
当然，这也是响应式的，上面这个例子实际上实使现了这个元素节点的title属性和vue实例的message属性保持一致。