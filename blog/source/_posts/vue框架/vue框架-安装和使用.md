---
title: vue框架-安装和使用
date: 2018-05-11 11:50:54
tags: vue
categories: Web
---

一、Vue框架简介

<!--more-->

Vue是一套用于构建用户界面的渐进式框架,可以自底向上逐层应用（只关注与视图层）。
1.特点：	
	
	轻量级 - 前端界面框架
	高效率；
	上手快；
	简单易学；
	文档全面简洁；
2.功能
	
	模版渲染；
	模块化;
	扩展功能:
		路由;
		Ajax;
3.作用：

	数据渲染/数据同步；
	组件化/模块化；
	其他功能：路由、ajax、数据流；
4.兼容：
	
	Vue不支持IE8及以下版本;
	Version:2.5.16
	
二、安装<br>	
1.[Vue Devtools](https://github.com/vuejs/vue-devtools#vue-devtools)

	使用Vue时，推荐浏览器上安装	Vue Devtools，它允许在一个更友好的界面中审查和调试Vue应用。
	在浏览器上安装Vue Devtools的方法:
		1.已安装 Node6+ 和 Npm3+ 
		2.下载 clone this repo https://github.com/vuejs/vue-devtools#vue-devtools
		3.终端:npm install
		4.终端:npm run build
2.使用&lt;script&gt;引入方式
	
	在.html文件中，通过下列方式
	<!-- 开发环境版本，包含了用帮助的命令行警告 -->
	<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
	或者
	<!-- 生产环境版本，优化了尺寸和速度 -->
	<script src="https://cdn.jsdelivr.net/npm/vue"></script>
	<!-- 手动更新的指定版本号 -->
	<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
3.NPM 
	
	在用Vue构建大型应用时推荐使用NPM安装
	命令:npm install vue
4.对不同构建版本的解释<br>
在NPM包的 dist/ 目录 包含许多不同的Vue.js构建版本<br>

||UMD|CommonJS|ES Module|
|---|---|---|---|
|完整版|vue.js|vue.common.js|vue.esm.js|
|只包含运行时版|vue.runtime.js|vue.runtime.common.js|vue.runtime.esm.js|
|完整版(生产环境)|vue.min.js|||
|只包含运行时版(生产环境)|vue.runtime.min.js|||

	完整版：同时包含编译器和运行时的版本;
	编译器：用来将模版字符串编译成JavaScript渲染函数的代码;
	运行时：用来创建Vue实例、渲染并处理虚拟DOM等代码。(去除编译器的其它一切)
	UMD：UMD版本可以通过<script>标签直接用在浏览器中
		jsDelivr CDN 的 https://cdn.jsdelivr.net/npm/vue 默认文件就是运行时 + 编译器的 UMD 版本 (vue.js)
	CommonJS：CommonJS 版本用来配合老的打包工具比如 Browserify 或 webpack 1
		这些打包工具的默认文件 (pkg.main) 是只包含运行时的 CommonJS 版本 (vue.runtime.common.js)
	ES Module：ES module 版本用来配合现代打包工具比如 webpack 2 或 Rollup
		这些打包工具的默认文件 (pkg.module) 是只包含运行时的 ES Module 版本 (vue.runtime.esm.js)
5.运行时+编译器 vs 只包含运行时

	如果你需要在客户端编译模板 (比如传入一个字符串给 template 选项，或挂载到一个元素上并以其 DOM 内部的 HTML 作为模板)，就将需要加上编译器，即完整版：
		// 需要编译器
		new Vue({
		  template: '<div>{{ hi }}</div>'
		})
		
		// 不需要编译器
		new Vue({
		  render (h) {
		    return h('div', this.hi)
		  }
		})

	当使用 vue-loader 或 vueify 的时候，*.vue 文件内部的模板会在构建时预编译成 JavaScript。你在最终打好的包里实际上是不需要编译器的，所以只用运行时版本即可		
	运行时版本相比完整版体积要小大约 30%，所以应该尽可能使用这个版本。如果你仍然希望使用完整版，则需要在打包工具里配置一个别名：
6.开发环境 vs 生产环境模式

	对于 UMD 版本来说，开发环境/生产环境模式是硬编码好的：开发环境下用未压缩的代码，生产环境下使用压缩后的代码。

	CommonJS 和 ES Module 版本是用于打包工具的，因此我们不提供压缩后的版本。你需要自行将最终的包进行压缩。

	CommonJS 和 ES Module 版本同时保留原始的 process.env.NODE_ENV 检测，以决定它们应该运行在什么模式下。你应该使用适当的打包工具配置来替换这些环境变量以便控制 Vue 所运行的模式。把 process.env.NODE_ENV 替换为字符串字面量同时可以让 UglifyJS 之类的压缩工具完全丢掉仅供开发环境的代码块，以减少最终的文件尺寸
7.开发版本
	
	git clone https://github.com/vuejs/vue.git node_modules/vue
	cd node_modules/vue
	npm install
	npm run build	
三、使用<br>

``` html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>菜鸟教程-Vue</title>
    <!-- <script src="http://static.runoob.com/assets/vue/1.0.11/vue.min.js"></script> -->
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
</head>
<body>
    <div id="app">
        {{message}}
    </div>

    <!-- JavaScript 代码需要放在尾部（指定的HTML元素之后） -->
    <script>
        new Vue({
            el: '#app',
            data: {
                message: 'Hello World Vue!'
            }
        });
    </script>
</body>
</html>

```
	
参考资料:		
1.[Vue.js中文官网](https://cn.vuejs.org)<br>
2.[Vuejs源码](https://github.com/vuejs/vue)<br>
3.[Vuejs官方工具](https://github.com/vuejs)<br>
4.[Vuejs官方论坛](https://forum.vuejs.org/)<br>
5.[Vuejs安装](https://cn.vuejs.org/v2/guide/installation.html)<br>
6.[Vue Devtools](https://github.com/vuejs/vue-devtools#vue-devtools)<br>
7.[Node.js官网](http://nodejs.cn)<br>
8.[Vue - runoob](http://www.runoob.com/w3cnote/vue-js-quickstart.html)

