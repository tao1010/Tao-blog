---
title: HBuilder入门-环境
toc: true
date: 2018-07-16 15:42:44
tags:  HBuilder基础
categories: HBuilder
---

一、HBuilder

<!-- more -->

1.概念
	
	主要用于开发html、js、css，同时配合HTML的后端脚本语言如php、jsp也可以适用，还有前端的预编译语言如less以及markdown都可以良好的编辑.
	HBuilder是基于eclipse 3.7 开发的，不兼容eclipse4.x版本的插件
2.快捷方式

		连续单词的首字母; dg-->document.get
		整段HTML一般使用tag的名称; sc/st-->script/style
		同一个tag，有多个代码块输出，则在最后加后缀;
			meta
			metau
		如果原始语法超过4个字符，针对常用语法，则第一个单词的激活符使用缩写;
			inbu--> input button
		js的关键字代码块，是在关键字最后加一个重复字母;
			iff	--> if(){}
			forr --> for(){}
			funn --> function代码块
			funa --> 函数
			func --> 闭包
3.JSDoc

	作用：导出API文档，明确代码类型，辅助代码提示；
	标签
		@alias  给变量和函数指定别名
		@constructor 标识函数是构造函数
		@description 代码提示时的描述
		@example 	提示代码示例
		@extends	标识继承于某个类型
		@param	描述参数和参数类型
		@property	描述对象的属性
4.扩展语法提示、自定义语法

	JS部分：
		1.基于sdocml的框架与法库
			选中工程，点击右键，引入框架语法；
			常用框架语法库，包括jquery、zepto、mui等
		2.jsdoc
		3.js代码块
	HTML部分：
		标签和属性
5.引入JQ等扩展框架语法库
	
	强烈不推荐使用：jquery mobile是非常卡顿的；
	在手机端的ui框架，推荐使用mui。
6.插件
	
	HBuilder的工具→插件安装中已经集成了svn、git、php等常用的插件；
二、HTML5 plus 和 Native.js    
1.HTML5+

	是HBuilder对原生组件的封装，主要展示HBuilder中对各类组件的用法(方法、对象、属性、权限等)
2.Native.js

	封装一条通道(通过JS语法直接调用Native API
		Android -- JAVA
		iOS -- Objective C
	Android调用方法：plus.android
	iOS调用方法：plus.ios
3.Native.js - iOS    
方法

对象

权限
	
	



三、5+App(Html 5 Plus app)
1.概念
	
	HTML5 Plus 移动app的简称5+App。
	
	mobile web项目在浏览器运行
	
	app
	
	mobile web 打包成 app
2.语言

	HTML、JS、CSS

3.打包

	本地打包
	云打包
	证书：
		android：使用DCloud生成的公用证书或自己生成的证书，两者不影响安装包的发布，唯一的差别就是证书中开发者和企业信息不同。
三、wap2app		
1.概念

	将移动站点，快速发布成App的增强方案；
四、流应用





参考资料:		
1.[5+ App开发Native.js入门指南](http://ask.dcloud.net.cn/article/88)    
2.[HBuilder架构](http://www.dcloud.io/)     
3.[HBuilder中的UI-mui](http://ask.dcloud.net.cn/article/91)     
  [MUI](http://www.dcloud.io/hellomui/)     
4.[HTML5+规范](http://www.html5plus.org/doc/h5p.html)     
5.[HTML5+ SDK](http://ask.dcloud.net.cn/docs/#//ask.dcloud.net.cn/article/104)     
6.[HBuilder-MUI](http://dev.dcloud.net.cn/mui/)     
7.[mui.init 和 plusready的区别](https://blog.csdn.net/u014466109/article/details/71311028)    
8.[Native.js - iOS](http://www.html5plus.org/doc/zh_cn/ios.html#plus.ios.importClass)
[Native.js - iOS/Android](http://ask.dcloud.net.cn/article/114)      
9.[HBuilder文档](http://ask.dcloud.net.cn/docs/)     

