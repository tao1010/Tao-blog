---
title: vue框架-基础-Class和Style绑定
date: 2018-06-15 14:44:41
tags: vue
categories: Web
---

	
	操作元素的 class 列表和内联样式是数据绑定的一个常见需求;
	在将 v-bind 用于 class 和 style 时，Vue.js 做了专门的增强。表达式结果的类型除了字符串之外，还可以是对象或数组;
1.绑定HTML Class

<!--more-->
	
	对象语法
		1.传给 v-bind:class 一个对象，以动态地切换 class
			<div v-bind:class="{ active: isActive }"></div>
		
		2.可以在对象中传入更多属性来动态切换多个 class
		
		3.v-bind:class 指令也可以与普通的 class 属性共存
			<div class="static"
			     v-bind:class="{ active: isActive, 'text-danger': hasError }">
			</div>
			
	数组语法
		可以把一个数组传给 v-bind:class，以应用一个 class 列表：
			<div v-bind:class="[activeClass, errorClass]"></div>
			
			data: {
			  activeClass: 'active',
			  errorClass: 'text-danger'
			}
		渲染为：
			<div class="active text-danger"></div>
		
	在组件上
		先学习Vue组件...
2.绑定内联样式

	对象语法
		v-bind:style 的对象语法十分直观——看着非常像 CSS，但其实是一个 JavaScript 对象。
		
		CSS 属性名可以用驼峰式 (camelCase) 或短横线分隔 (kebab-case，记得用单引号括起来) 来命名
			<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
			 
			 data: {
			 	activeColor: 'red',
			 	fonSize: 30
			 }
		直接绑定样式对象(是的模版更清晰):
			<div v-bind:style="styleObject"></div>
			
			data: {
				styleObject: {
					color: 'red',
					fontSize: '13px'
				}
			}

	数组语法
		v-bind:style 的数组语法可以将多个样式对象应用到同一个元素上：
			<div v-bind: style="[baseStyle, overridingStyles]"></div>

	自动添加前缀
		当 v-bind:style 使用需要添加浏览器引擎前缀的 CSS 属性时，如 transform，Vue.js 会自动侦测并添加相应的前缀
		
	多重值(2.3.0+)
		可以为 style 绑定中的属性提供一个包含多个值的数组，常用于提供多个带前缀的值:
			<div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>
			解析:
				这样写只会渲染数组中最后一个被浏览器支持的值。在本例中，如果浏览器支持不带浏览器前缀的 flexbox，那么就只会渲染 display: flex
				
参考资料:<br>
1.[Vue.js中文官网](https://cn.vuejs.org)<br>
