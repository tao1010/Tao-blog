---
title: vue框架-基础和实践
date: 2018-06-14 14:21:57
tags: vue
categories: Web
---

一、Vue实例<br>
1.创建Vue实例<br>
	
	每个 Vue 应用都是通过用 Vue 函数创建一个新的 Vue 实例开始的;
		var vm = new Vue({
			//选项
		})
	文档中经常会使用 vm (ViewModel 的缩写) 这个变量名表示 Vue 实例;
	一个 Vue 应用由一个通过 new Vue 创建的根 Vue 实例，以及可选的嵌套的、可复用的组件树组成;
		根实例
		└─ TodoList
		   ├─ TodoItem
		   │  ├─ DeleteTodoButton
		   │  └─ EditTodoButton
		   └─ TodoListFooter
		      ├─ ClearTodosButton
		      └─ TodoListStatistics
	所有的 Vue 组件都是 Vue 实例，并且接受相同的选项对象 (一些根实例特有的选项除外);
2.数据和方法<br>
	
	当一个 Vue 实例被创建时，它向 Vue 的响应式系统中加入了其 data 对象中能找到的所有的属性。当这些属性的值发生改变时，视图将会产生“响应”，即匹配更新为新的值;
	只有当实例被创建时 data 中存在的属性才是响应式的;
	如果在晚些时候需要一个属性，但是一开始它为空或不存在，那么仅需要设置一些初始值;
例外:

```html
<body>
    <div id="app">
        <p>{{ foo }}</p>
        <!-- 这里的 `foo` 不会更新！ -->
        <button v-on:click="foo = 'baz'">Change it</button>
    </div>

    <!-- JavaScript 代码需要放在尾部（指定的HTML元素之后） -->
    <script>

        var obj = {
            foo: 'bar'
        }
        Object.freeze(obj)


        new Vue({
            el: '#app',
            data: obj
        });
    </script>
</body>
```
	
	解析：
		使用 Object.freeze()，这会阻止修改现有的属性，也意味着响应系统无法再追踪变化;
Vue实例还暴露了实例属性和方法
	
	它们前缀 $ ，以便与用户定义的属性区分开;
	eg: 
		var data = {a: 1}
		var vm = new Vue({
			el: '#example',
			data: data
		})
		 
		vm.$data === data //true
		vm.$el === document.getElementById('example')//ture
		vm.$watch('a', function(newValue,oldValue) {
			//这个回调将在'vm.a'改变后调用
		})
3.实例生命周期钩子<br>	

	每个Vue实例在被创建时都要经过一系列初始化过程：
		设置数据监听
		编译模版
		将实例挂载到DOM
		数据变化时更新DOM
		...等
	生命周期钩子:
		用户在不同阶段添加自己代码的时机.
	Vue实例生命周期钩子：
		created - 创建
		mounted - 挂载到DOM
		updated - 更新DOM
		destroyed - 销毁
eg:

```html
<body>
    <div id="app">
        <p>{{ message }}</p>
        <button v-on:click="getMessage">Change it</button>
    </div>

    <!-- JavaScript 代码需要放在尾部（指定的HTML元素之后） -->
    <script>
        new Vue({
            el: '#app',
            data: {
                message: 'Hello Vue!'
            },
            created: function(){
                console.log('created')
            },
            methods: {
                getMessage: function (){
                    this.message = 'haha'
                }
            }
        });
    </script>
</body>
```
tips:

	不要在选项属性或回调上使用箭头函数，比如 created: () => console.log(this.a) 或 vm.$watch('a', newValue => this.myMethod())
	原因：
		箭头函数是和父级上下文绑定在一起的，this 不会是如你所预期的 Vue 实例，经常导致 Uncaught TypeError: Cannot read property of undefined 或 Uncaught TypeError: this.myMethod is not a function 之类的错误
4.生命周期图示<br>
![lifecycle](lifecycle.png)<br>
二、模版语法<br>
1.插值<br>
文本<br>
	
	数据绑定最常见的形式:使用 "Mustache" 语法 - 双大括号{{}}的文本插值
	eg1: - 实时更新
		<span>Message: {{ msg }}</span>
	解析1：
		只要绑定的数据对象 msg属性发生改变，插值处的内容都会更新;
	eg2: - 一次更新
		<span v-once>这个将不会改变: {{ msg }}</span>
	解析2:
		通过使用指令v-once，执行一次性插值，当数据改变时，插值处的内容不会更新;
原始HTML<br>
	
	双大括号会将数据解释为普通文本，而非 HTML 代码。为了输出真正的 HTML，你需要使用 v-html 指令：

```html
<body>
    <div id="app">
        <p>{{ message }}</p>
        <p v-once>{{ once }}</p>
        <button v-on:click="getMessage">Change it</button>
        <p>Using mustaches:{{rawHtml}}</p>
        <p>Using v-html directive:<span v-html='rawHtml'></span></p>
    </div>

    <!-- JavaScript 代码需要放在尾部（指定的HTML元素之后） -->
    <script>
        var vm = new Vue({
            el: '#app',
            data: {
                message: 'Hello Vue!',
                once: '一次性插值',
                rawHtml: "<span style='color:red'>This should be red."
            },
            created: function(){
                console.log('created')
            },
            methods: {
                getMessage: function (){
                    this.message = 'haha'
                }
            }
        });
    </script>
</body>
```
		
<font color=red>站点上动态渲染的任意 HTML 可能会非常危险，因为它很容易导致 XSS 攻击。请只对可信内容使用 HTML 插值，绝不要对用户提供的内容使用插值</font><br>
特性<br>
	
	Mustache 语法不能作用在 HTML 特性上，遇到这种情况应该使用 v-bind 指令：
		<div v-bind:id="dynamicId"></div>
	
	布尔值特性:
		<button v-bind:disabled="isButtonDisabled">Button</button>
	解析：
		如果 isButtonDisabled 的值是 null、undefined 或 false，则 disabled 特性甚至不会被包含在渲染出来的 <button> 元素中
使用JavaScript表达式<br>
	
	每个绑定都只能包含单个表达式;
	模板表达式都被放在沙盒中，只能访问全局变量的一个白名单，如 Math 和 Date ;
	不应该在模板表达式中试图访问用户定义的全局变量。
	
2.指令<br>

	指令 (Directives) 是带有 v- 前缀的特殊特性。指令特性的值预期是单个 JavaScript 表达式 (v-for 是例外情况，稍后我们再讨论)。
	指令的职责是，当表达式的值改变时，将其产生的连带影响，响应式地作用于 DOM.
参数
	
	一些指令能够接收一个“参数”，在指令名称之后以冒号表示:
	参数是：href
		<a v-bind:href="url">...</a>
	参数是：监听的事件名
		<a v-on:click="doSomething">...</a>
修饰符

	修饰符 (Modifiers) 是以半角句号 . 指明的特殊后缀，用于指出一个指令应该以特殊方式绑定。
		<form v-on:submit.prevent="onSubmit">...</form>
3.缩写<br>
	
	Vue.js为最常用的指令提供简写：
		v-bind
		完整：<a v-bind:href='url'>...</a>
		简写：<a :href='url'>...</a>
		v-on
		完整：<a v-on:click='doSomething'>...</a>
		简写：<a @click='doSomething'>...</a>
三、计算属性和侦听器<br>

四、Class和Style绑定<br>

五、条件渲染<br>

六、列表渲染<br>

七、事件处理<br>

八、表单输入绑定<br>



参考资料:<br>
1.[Vue.js中文官网](https://cn.vuejs.org)<br>