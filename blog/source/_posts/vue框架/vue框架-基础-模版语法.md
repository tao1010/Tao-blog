---
title: vue框架-基础-模版语法
date: 2018-06-14 14:39:08
tags: vue
categories: Web
---

1.插值<br>

<!--more-->

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
		
	eg：
	<body>
	    <div id="app">
	        <p>{{ok? 'Yes': 'NO'}}</p>
	        <p v-if='seen'>现在看见你了</p>
	        <a v-bind:href="url">百度</a>
	        <a v-on:click='doSomething'>doSomething</a>
	    </div>
	
	    <!-- JavaScript 代码需要放在尾部（指定的HTML元素之后） -->
	    <script>
	        var vm = new Vue({
	            el: '#app',
	            data: {
	                url: 'https://www.baidu.com',
	                seen: true,
	                ok: true,
	            },
	            created: function(){
	                console.log('created')
	            },
	            methods: {
	                getMessage: function (){
	                    this.message = 'haha'
	                },
	                doSomething: function (){
	                    this.url = 'https://cn.vuejs.org'
	                }
	            }
	        });
	    </script>
	</body>
	
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

参考资料:<br>
1.[Vue.js中文官网](https://cn.vuejs.org)<br>