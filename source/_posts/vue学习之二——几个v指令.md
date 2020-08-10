---
title: vue学习之二——几个v指令
date: 2020-02-02 16:08:44
categories:
	- fe
tags:
	- vue
---
## 条件v-if，循序v-for
控制一个元素是否显示需要用到v-if，如下：
```
<div id="app-3">
  <p v-if="seen">现在你看到我了</p>
</div>
```
```
var app3 = new Vue({
  el: '#app-3',
  data: {
    seen: true
  }
})
```
seen属性改为false就不显示这个元素了。<br>
<!-- more -->
所以vue不仅可以把数据绑定到DOM和属性，还可以绑定到DOM结构。<br>
常和条件放在一起说的就是循环，vue循环用v-for指令，可绑定数组的数据来渲染一个项目列表：
```
<div id="app-4">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>
```
```
var app4 = new Vue({
  el: '#app-4',
  data: {
    todos: [
      { text: '学习 JavaScript' },
      { text: '学习 Vue' },
      { text: '整个牛项目' }
    ]
  }
})
```
## 事件监听
总不能用户都在console里操作数据吧？用户操作后我们如何实现处理呢？这里就有一个监听的概念了。<br>
为了让用户和你的应用进行交互，我们可以用 v-on 指令添加一个事件监听器，通过它调用在 Vue 实例中定义的方法：
```
<div id="app-5">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">反转消息</button>
</div>
```
```
var app5 = new Vue({
  el: '#app-5',
  data: {
    message: 'Hello Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
```
这里其实是监听了click的操作，在vue示例里调用自己实现的操作自己实例属性的方法，由于实例的属性和DOM文本是绑定的，这种响应式的交互会改变message的显示。
怎么样，是不是串起来了？
## 双向绑定v-model
Vue 还提供了 v-model 指令，它能轻松实现表单输入和应用状态之间的双向绑定。
```
<div id="app-6">
  <p>{{ message }}</p>
  <input v-model="message">
</div>
```
```
var app6 = new Vue({
  el: '#app-6',
  data: {
    message: 'Hello Vue!'
  }
})
```
上面的代码可以实现input元素和p元素保持一致，即无论是修改输入值还是操作vue示例修改message值进而改到p标签message值，都会互相影响。
