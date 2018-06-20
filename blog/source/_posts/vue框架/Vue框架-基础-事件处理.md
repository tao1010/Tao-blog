---
title: Vue框架-基础-事件处理
toc: true
date: 2018-06-20 13:31:20
tags: vue
categories: Web
---

1.监听事件

<!-- more -->

v-on 可以接收JavaScript代码<br>
指令监听DOM事件，并在触发时运行JavaScript：
	
	<div id="app7">
        <button v-on:click=" counter += 1">+ 1</button>
        <p>The button above has been clicked {{ counter}} times.</p>
    </div>
    
    var vm7 = new Vue({
        el: '#app7',
        data: {
            counter: 0
        }
    });
2.事件处理方法<br>
v-on 接收需要调用的方法名称：

	<div id="app8">
        <button v-on:click='greet'>Greet</button>
    </div>
    
    
    var vm8 = new Vue({
        el: '#app8',
        data: {
            name: 'Vue.js'
        },
        // 在 ‘method’ 对象中定义方法
        methods: {
            greet: function(event){
                // ‘this’ 在方法里指向当前Vue实例
                alert('Hello ' + this.name + '!')
                // 'event' 是原生DOM事件
                if(event){
                    alert(event.target.tagName)
                }
            }
        }
    });
用于处理逻辑复杂场景！<br>
3.内联处理器中的方法<br>
v-on 可以在内联的JavaScript中调用方法：

	<div id="app9">
        <button v-on:click="say('hi')">Say Hi</button>
        <button v-on:click="say('what')">Say what</button>
        <button v-on:click="warn('Form cannot be submitted yet.',$event)">Submit</button>
    </div>
    
    var vm9 = new Vue({
        el: "#app9",
        methods: {
            say: function(message){
                alert(message)
            },
            warn: function(message,event){
                if(event){
                    event.preventDefault()
                }
                alert(message)
            }
        }
    });
v-on 有时需要在内联语句处理器中访问原始的DOM事件，可以用特殊变量$event 把它传入方法：

	eg:上述‘Submit’ 按钮
4.事件修饰符<br>
常用的需求处理方法：

	event.preventDefault()
	event.stopPropagation()
	
	修饰符是由点开头的指令后缀来表示：
		.stop
		.prevent
		.capture
		.self
		.once
		.passive - 尤其能够提升移动端的性能
	
	eg:
		<!-- 阻止单击事件继续传播 -->
		<a v-on:click.stop="doThis"></a>
		
		<!-- 提交事件不再重载页面 -->
		<form v-on:submit.prevent="onSubmit"></form>
		
		<!-- 修饰符可以串联 -->
		<a v-on:click.stop.prevent="doThat"></a>
		
		<!-- 只有修饰符 -->
		<form v-on:submit.prevent></form>
		
		<!-- 添加事件监听器时使用事件捕获模式 -->
		<!-- 即元素自身触发的事件先在此处处理，然后才交由内部元素进行处理 -->
		<div v-on:click.capture="doThis">...</div>
		
		<!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
		<!-- 即事件不是从内部元素触发的 -->
		<div v-on:click.self="doThat">...</div>
		
		<!-- 滚动事件的默认行为 (即滚动行为) 将会立即触发 -->
		<!-- 而不会等待 `onScroll` 完成  -->
		<!-- 这其中包含 `event.preventDefault()` 的情况 -->
		<div v-on:scroll.passive="onScroll">...</div>

修饰符可用串联，顺序不同结果不同：

	v-on:click.prevent.self 会阻止所有的点击
	v-on:click.self.prevent 只会阻止对元素自身的点击
tips:
	
	不要把 .passive 和 .prevent 一起使用，因为 .prevent 将会被忽略，同时浏览器可能会向你展示一个警告。
	.passive 会告诉浏览器你不想阻止事件的默认行为;
5.按键修饰符<br>
Vue允许为 v-on 在监听键盘事件时添加按键修饰符：

	eg：
		<!-- 只有在 `keyCode` 是 13 时调用 `vm.submit()` -->
		<input v-on:keyup.13="submit">

	全部按键别名：
		.enter
		.tab
		.esc
		.space
		.up
		.down
		.left
		.right
可以通过全局 config.keyCodes 对象自定义按键修饰符别名：

	// 可以使用 `v-on:keyup.f1`
	Vue.config.keyCode.f1 = 12
自动匹配按键修饰符

	可直接将 KeyboardEvent.key 暴露的任意有效按键名转换为 kebab-case 来作为修饰符：
		<input @keyup.page-down="onPageDown">
		解析：处理函数仅在 $event.key === 'PageDown' 时被调用
有一些按键 (.esc 以及所有的方向键) 在 IE9 中有不同的 key 值, 如果你想支持 IE9，它们的内置别名应该是首选。<br>
6.系统修饰键<br>
实现仅在按下相应按键时才触发鼠标或键盘事件的监听器：
	
	.ctrl
	.alt
	.shift
	.meta
	.exact 修饰符允许你控制由精确的系统修饰符组合触发的事件 （v2.5.0 add）
	
	鼠标按钮修饰键 v2.2.0 add
		.left
		.right
		.middle
	这些修饰符会限制处理函数仅响应特定的鼠标按钮
tips：	

	不同系统键盘的按键的区别；
	修饰键与常规按键不同，在和 keyup 事件一起用时，事件触发时修饰键必须处于按下状态。换句话说，只有在按住 ctrl 的情况下释放其它按键，才能触发 keyup.ctrl。而单单释放 ctrl 也不会触发事件。如果你想要这样的行为，请为 ctrl 换用 keyCode：keyup.17；
7.在HTML中监听事件<br>
v-on的优势：
	
	扫一眼 HTML 模板便能轻松定位在 JavaScript 代码里对应的方法；
	无须在 JavaScript 里手动绑定事件，ViewModel 代码可以是非常纯粹的逻辑，和 DOM 完全解耦，更易于测试；
	当一个 ViewModel 被销毁时，所有的事件处理器都会自动被删除。无须担心如何自己清理它们;

参考资料:<br>
1.[Vue.js中文官网](https://cn.vuejs.org)<br>