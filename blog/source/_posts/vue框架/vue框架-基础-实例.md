---
title: vue框架-基础-实例
date: 2018-06-14 14:21:57
tags: vue
categories: Web
---

1.创建Vue实例<br>
	
<!--more-->	
	
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

参考资料:<br>
1.[Vue.js中文官网](https://cn.vuejs.org)<br>