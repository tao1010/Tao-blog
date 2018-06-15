---
title: vue框架-条件渲染
date: 2018-06-15 14:45:59
tags: vue
categories: Web
---

1.v-if
	
	1.v-if 是一个指令，必须将它添加到一个元素上
	eg1：
		<h1 v-if="isOK">YES</h1>
		<h1 v-else>NO</h1>
	2.在<template>元素上使用v-if条件渲染分组
	eg2：
		 <div id="app1">
	        <template v-if='isOK'>
	            <h1>菜鸟教程-Vue</h1>
	            <p>条件分组渲染1</p>
	            <p>条件分组渲染2</p>
	        </template>
	    </div>
	3.v-else
	可以使用 v-else 指令来表示 v-if 的“else 块”;
	必须紧跟在带 v-if 或者 v-else-if 的元素的后面，否则它将不会被识别;
	eg3:
		<div v-if="Math.random() > 0.5">
		  Now you see me
		</div>
		<div v-else>
		  Now you don't
		</div>
	4.v-else-if
	类似于 v-else，v-else-if 也必须紧跟在带 v-if 或者 v-else-if 的元素之后;
	
	5.用key管理可复用的元素
	eg：
		 HTML
		  <div id="app2">
	        <template v-if="loginType === 'username'">
	            <label>UserName</label>
	            <input placeholder="请输入您的姓名" key="username-input">
	        </template>
	        <template v-else>
	            <label>Email</label>
	            <input placeholder="请输入您的邮箱" key="email-input">
	        </template>
	        <div>
	            <button v-on:click='changeType'>切换</button>
	        </div>
	    </div>	
	    
	    JS
		  var vm2 = new Vue({
            el: '#app2',
            data: {
                loginType: 'username'
            },
            methods: {

                changeType: function(){
                    if(this.loginType === 'username'){
                        this.loginType = 'hello'
                    }else{
                        this.loginType = 'username'
                    }
                }
            }
        });	
2.v-show
	
	用于根据条件展示元素的选项是 v-show 指令;
	不同的是带有 v-show 的元素始终会被渲染并保留在 DOM 中。
	v-show 只是简单地切换元素的 CSS 属性 display;
	v-show 不支持 <template> 元素，也不支持 v-else;
	eg:
		<h2 v-show='isOK'>Hello Vue!</h2>
	
	
3.v-if VS v-show

	v-if
		是“真正”的条件渲染 - 会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建;
		是惰性的 - 如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块;
	v-show
		不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换;
	v-if 有更高的切换开销，而 v-show 有更高的初始渲染开销;
	需要频繁地切换，使用v-show较好;
	运行时条件很少改变，使用v-if较好;
	
4.v-if 与v-for一起使用
	
	当 v-if 与 v-for 一起使用时，v-for 具有比 v-if 更高的优先级;

参考资料:<br>
1.[Vue.js中文官网](https://cn.vuejs.org)<br>
2.[列表渲染指南](https://cn.vuejs.org/v2/guide/list.html#v-for-with-v-if)<br>