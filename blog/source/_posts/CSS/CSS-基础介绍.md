---
title: CSS-基础介绍
date: 2018-03-15 09:18:28
tags: CSS
categories: Web

---

一、CSS(层叠样式表)简介

1.CSS概念

CSS - 是一种用于向用户指定文档如何呈现的语言--它们如何指定样式、布局等。

文档 - 通常是用标记语言结构化的文本文件--HTML是最常用的标记语言，其他标记语言如SVG、XML等。

呈现 - 文档给用户意味着将其转换为用户可用的形式。可视化呈现文档：Firefox、chrome、IE浏览器等，计算机屏幕、投影仪、打印机等。


CSS不是真正的编程语言，它是样式语言，允许你有选择地为HTML文档的元素添加样式。

CSS用于设计风格和布局（更改内容字体、颜色、大小、间距，将内容分为多列，添加动画，其他装饰效果）。（HTML用于定义内容的结构和语义）


2.CSS对HTML的影响

一个CSS规则由以下组成：

	一组属性：属性的值更新了 HTML 的内容的显示方式。比如，我想让元素的宽度是其父元素的50％，或者元素背景变为红色。
	
	一个选择器：它选择元素，这（些）元素是你想应用这些最新的属性值于其上的元素。比如，我想将我的CSS规则应用到我HTML文档中的所有段落上。
	
样式表中的一组CSS规则确定了网页如何显示。

二、CSS的使用

![CSS-parsing](CSS-parsing.png)

1.CSS在HTML的引用方法
在html文件中，head之中加入下列代码：

	<head>
		...
		<link rel="stylesheet" href="styles/style.css" type="text/css">
	</head>
	
	<body>
		<h1>这是一个CSSDemo标题</h1>
   		<p>This is my first CSS example</p>
	</body>
	
保存html文件，浏览器打开即可生效.

2.解析CSS

	h1{
		  color: blue;//将文本颜色设置为蓝色；
		  background-color: yellow;//将背景颜色设置为黄色；
		  border:1px solid black;//将标题（h1是标题元素）边框（border）设置为：1像素宽、实线（不是虚线、点线等）、颜色黑色。
	}
	p{
	  	  color: red;//设置字体颜色为红色
	}
第一条规则从 h1 选择器开始，这意味着它将其属性值应用到&lt;h1&gt;元素上，它包含三个属性和属性各自的值（每个 属性/值 对称为一个声明；

第二个规则从 p 选择器开始，这意味着它将其属性值应用到&lt;p&gt;元素上。

三、CSS工作原理

![CSS-work-principle](CSS-work-principle.png)

当浏览器显示文档时，它必须将文档的内容与其样式信息结合。它分两个阶段处理文档：

	1.浏览器将 HTML 和 CSS 转化成 DOM （文档对象模型）。DOM在计算机内存中表示文档。它把文档内容和其样式结合在一起。
	
	2.浏览器显示 DOM 的内容。


四、关于DOM

1.DOM - 文档对象模型(Document Object Model)是一种树形结构。标记语言中的每个元素、属性、文本片段都变为一个 DOM 节点。这些节点由它们与其它 DOM 节点的关系来定义。有的元素是某些子节点的父节点，且这些子节点有兄弟（节点）。

2.表示DOM

	<p>
		  Let's use:
		  <span>Cascading</span>
	  	  <span>Style</span>
	  	  <span>Sheets</span>
	</p>

在该 DOM 中，我们的&lt;p&gt;元素所对应的节点是父节点。

&lt;p&gt;元素的子节点:一个文本节点 + &lt;span&gt;元素对应的节点。

这些 SPAN 结点也是父节点，子节点：它们各自的文本节点。

	P
	├─ "Let's use:"
	├─ SPAN
	|  	└─ "Cascading"
	├─ SPAN
	|  └─ "Style"
	└─ SPAN
   		└─ "Sheets"

3.应用CSS到DOM

	span {
	  	border: 1px solid black;
	  	background-color: lime;
	}
	
浏览器会解析 HTML 并通过它创建 DOM，之后解析 CSS。由于 CSS 只有一个可用的规则，该规则有一个span选择器，它会将这个规则应用到这三个<span>的每一个上。


五、将CSS应用到HTML的方式：

1.<font color=red>外部样式表：此方法最佳</font>(可以使用一个样式表来设置多个文档的样式，需要更新CSS时只要在一个地方更新)

当你将你的CSS保存在一个独立的扩展名为.css的文件中，并从HTML的&lt;link&gt;元素中引用它。

2.内部样式表：

指不使用外部CSS文件，将你的CSS放置在&lt;style&gt;元素中，该元素包含在HTML的head内:
	
	<html>
		<head>
			...
			<style>
				...
				h1{
					border:1px solid black;
				}
				p{
					color:red;
				}
			</style>
		</head>
		<body>
			...
		</body>
	</html>
在某些情况有用（正在使用内容管理系统，不能修改CSS文件）;但它不如外部样式表高效 —— 在网站中，CSS 将需要在每个页面重复，并且需要更新时要更改的多个位置。

3.内联样式：仅影响一个元素的CSS声明，被style属性包括着

	<!DOCTYPE html>
	<html>
		  <head>
			    <meta charset="utf-8">
			    <title>My CSS experiment</title>
		  </head>
		  <body>
			    <h1 style="color: blue;background-color: yellow;border: 1px solid black;">Hello World!</h1>
			    <p style="color:red;">This is my first CSS example</p>
		  </body>
	</html>

在非必要，否则不要这么做！这很难维护（你可能不得不在每份文档里更新多次同样的信息），并且它还混合了 CSS 表示的样式信息和 HTML 的结构信息，使 CSS 难以阅读和理解。

保持不同类型代码的分离和纯净使处理该代码的任何人工作更为容易。


参考资料：

1.[CSS如何工作](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Introduction_to_CSS/How_CSS_works)