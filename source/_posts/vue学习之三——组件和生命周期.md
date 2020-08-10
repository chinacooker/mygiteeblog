---
title: vue学习之三——组件和生命周期
date: 2020-02-03 10:18:25
categories:
	- fe
tags:
	- vue
---
## 组件
“组件系统是vue的另一个重要概念”，这里我们先记住它是抽象的，我们通常用小型、独立和通常可复用的组件来构建大型应用。<br>
在 Vue 里，一个组件本质上是一个拥有预定义选项的一个 Vue 实例。在 Vue 中注册组件很简单：
```
// 定义名为 todo-item 的新组件
Vue.component('todo-item', {
  template: '<li>这是个待办项</li>'
})

var app = new Vue(...)
```
现在你可以用它构建另一个组件模板：
```
<ol>
  <!-- 创建一个 todo-item 组件的实例 -->
  <todo-item></todo-item>
</ol>
```
上面只是个简单的例子，实际上我们应该能从父作用域将数据传到子组件，即父组件不定义实际的数据，从vue实例中传出，在DOM元素中绑定vue实例和组件即可，如下,我们可以使用 v-bind 指令将待办项传到循环输出的每个组件中：
<!-- more -->
```
<div id="app-7">
  <ol>
    <!--
      现在我们为每个 todo-item 提供 todo 对象
      todo 对象是变量，即其内容可以是动态的。
      我们也需要为每个组件提供一个“key”，稍后再
      作详细解释。
    -->
    <todo-item
      v-for="item in groceryList"
      v-bind:todo="item"
      v-bind:key="item.id"
    ></todo-item>
  </ol>
</div>
```
```
Vue.component('todo-item', {
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})

var app7 = new Vue({
  el: '#app-7',
  data: {
    groceryList: [
      { id: 0, text: '蔬菜' },
      { id: 1, text: '奶酪' },
      { id: 2, text: '随便其它什么人吃的东西' }
    ]
  }
})
```
## 生命周期和钩子
每个 Vue 实例在被创建时都要经过一系列的初始化过程——例如，需要设置数据监听、编译模板、将实例挂载到 DOM 并在数据变化时更新 DOM 等，这个就是生命周期了。<br>
同时在这个过程中也会运行一些叫做生命周期钩子的函数，这给了用户在不同阶段添加自己的代码的机会。<br>
钩子很重要（公司的开发跟我说的），我们列一下有哪些重要的钩子：
beforeCreate，created，beforeMount，mounted，beforeUpdate，updated，beforeDestroy，destroyed
先了知道有这么个东西吧~~