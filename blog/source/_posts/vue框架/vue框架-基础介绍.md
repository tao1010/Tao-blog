---
title: vue框架-基础介绍
date: 2018-06-14 11:22:51
tags: vue
categories: Web
---

1.声明式渲染<br>

```html
<head>
    <meta charset="UTF-8">
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
</head>
<body>
	
    <!-- 1.文本插值 -->
    <div id="app">
        {{message}}
    </div>

    <!-- 2.绑定元素特性 -->
    <div id="app-2">
        <span v-bind:title='message'>鼠标悬停几秒钟查看此处动态绑定的提示信息!</span>
    </div>
    <!-- JavaScript 代码需要放在尾部（指定的HTML元素之后） -->
    <script>
        new Vue({
            el: '#app',
            data: {
                message: 'Hello World Vue!'
            }
        });
        var app2 = new Vue({
            el: '#app-2',
            data: {
                message : '页面加载于' + new Date().toLocaleDateString()
            }
        });
    </script>
</body>
```

	解析：
	1.	Vue做了大量的背后工作;
		数据和DOM已经建立了关联;
		所以东西都是响应式的;
	2.	v-bind 特性被称为指令；
			实例中 v-bind:title='message'  表示：将这个元素节点的 title 特性和 Vue 实例的 message 属性保持一致
		指令带有前缀 v-，表示它们是Vue提供的新特性；
		浏览器的 JavaScript 控制台，输入 app2.message = '新消息'，鼠标放到文本上 就会看到这个绑定了 title 特性的 HTML 已经进行了更新
2.条件与循环<br>

```html
<body>
    <!-- 3.条件与循环 -->
    <div id="app-3">
        <p v-if='seen'>现在你看到我了</p>
    </div>
	<div id="app-4">
        <ol>
            <li v-for="todo in todos">{{ todo.text }}</li>
        </ol>
    </div>
    <!-- JavaScript 代码需要放在尾部（指定的HTML元素之后） -->
    <script>
        var app3 = new Vue({
            el: '#app-3',
            data: {
                seen: true
            }
        });
        var app4 = new Vue({
            el: '#app-4',
            data: {
                todos: [
                    {text: '学习JavaScript'},
                    {text: '学习Vue'},
                    {text: '整个牛项目'}
                ]
            }
        });
    </script>
</body>
```

	解析:
		控制台输入 app3.seen = false，显示的消息消失;
		控制台里，输入 app4.todos.push({ text: '新项目' })，列表最后添加了一个新项目；
		
	v-for 指令可以绑定数组的数据来渲染一个项目列表;
	可以把数据绑定到 DOM 文本或特性，还可以绑定到 DOM 结构;
	Vue 也提供一个强大的过渡效果系统，可以在 Vue 插入/更新/移除元素时自动应用过渡效果
3.处理用户输入<br>

```html
<body>
    <!-- 4.处理用户输入 -->
    <div id="app-5">
        <p>{{ message }}</p>
        <button v-on:click="reverseMessage">逆转消息</button>
    </div>
    <div id="app-6">
        <p>{{ message}}</p>
        <input v-model='message'>
    </div>
    <!-- JavaScript 代码需要放在尾部（指定的HTML元素之后） -->
    <script>
        var app5 = new Vue({
            el: '#app-5',
            data: {
                message: 'Hello Vue.js!'
            },
            methods: {
                reverseMessage: function(){
                    this.message = this.message.split('').reverse().join('')
                }
            }
        });
        var app6 = new Vue({
            el: '#app-6',
            data: {
                message: 'Hello Vue!'
            }
        });
    </script>
</body>
```
	
	解析:
		可以用 v-on 指令添加一个事件监听器，通过它调用在 Vue 实例中定义的方法;
		在 reverseMessage 方法中，我们更新了应用的状态，但没有触碰 DOM——所有的 DOM 操作都由 Vue 来处理，你编写的代码只需要关注逻辑层面即可;
		Vue 还提供了 v-model 指令，它能轻松实现表单输入和应用状态之间的双向绑定
4.组件化应用构建<br>

	组件系统是 Vue 的另一个重要概念，因为它是一种抽象，允许我们使用小型、独立和通常可复用的组件构建大型应用;
	几乎任意类型的应用界面都可以抽象为一个组件树;
	在 Vue 里，一个组件本质上是一个拥有预定义选项的一个 Vue 实例;

```html
<body>
    <!-- 4.组件 - 4.2 -->
    <div id="app-7">
        <ol>
                    <!--
            现在我们为每个 todo-item 提供 todo 对象
            todo 对象是变量，即其内容可以是动态的。
            我们也需要为每个组件提供一个“key”，稍后再
            作详细解释。
            -->
            <todo-item  v-for='item in groceryList'
                        v-bind:todo='item'
                        v-bind:key='item.id'>
            </todo-item>
        </ol>
    </div>

    <!-- JavaScript 代码需要放在尾部（指定的HTML元素之后） -->
    <script>

        // 组件化构建 - 4.1注册
        Vue.component('todo-item', {
            // todo-item 组件现在接受一个
            // "prop"，类似于一个自定义特性。
            // 这个 prop 名为 todo。
            props: ['todo'],
            template: '<li>{{ todo.text }}</li>'
        });

        //4.3
        var app7 = new Vue({
            el: '#app-7',
            data: {
                groceryList: [
                    {id: 0, text: '蔬菜'},
                    {id: 1, text: '奶酪'},
                    {id: 2, text: '随便其它什么人吃的东西'}
                ]
            }
        });
    </script>
</body>
```

	解析:
		子单元通过 prop 接口与父单元进行了良好的解耦;
		可以进一步改进 <todo-item> 组件，提供更为复杂的模板和逻辑，而不会影响到父单元;
在一个大型应用中，有必要将整个应用程序划分为组件，以使开发更易管理;

5.自定义组件和自定义元素的关系:
	
	Vue组件类似于自定义元素,它是Web组件规范的一部分；
	关键差别:
		Web组件规范处于草案阶段，未被所有浏览器原生实现；
		Vue组件不需要任何polfill，所有浏览器表现一致；
		Vue组件可包装于原生自定义元素内；
		Vue组件提供纯自定义元素所不具备的功能:
			跨组件数据流、自定义事件通信、构建工具集成.			
			
参考资料:<br>
1.[Vue.js中文官网](https://cn.vuejs.org)<br>