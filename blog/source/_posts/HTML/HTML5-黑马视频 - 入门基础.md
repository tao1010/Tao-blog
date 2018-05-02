---
title: HTML5-入门基础-黑马视频
date: 2018-05-02 09:20:11
tags: html5
categories: Web
---

一、浏览器内核		
	
	Webkit
	Blink
	Trident
	Gecko
二、Web标准		
	
	结构标准 XML+ XHTML
	样式标准 CSS
	行为标准 JS(DOM + ECMAScript)

三、HTML(Hyper Text Markup Language - 超文本标签语言)	
1.标签符号: &lt;...&gt; &lt;/...&gt;		
2.html骨架格式				
![demo1](demo1.png)				

```html
<html lang="en">
	<head>
	  <meta charset="UTF-8">
  	  <title>黑马视频 - Demo1</title>
     <link rel="stylesheet" href="html5-demo1.css">
	</head>
	<body>
	  ...
	  <script src="html5-demo1.js"></script>
	</body>
</html>

解析：	
	1.html标签 - 作用:所有html中标签的一个根标签;
	2.head标签 - 作用:用于存放title,meta,base,style,script,link注意再head标签中必须要设置的标签时title。
	3.title标签 - 作用:让页面拥有一个属于自己的标题;
	4.body标签 - 作用：页面的主体部分，用与存放所有的html标签(p、h、a、b、u、i、s、em、del、ins、strong、img)。
```
3.标签分类			
在HTML页面中,带有“&lt;&gt;”符号的元素被称为HTML标签

	双标签： <标签名>标签内容</标签名>
		eg:<body>这是主体标签</body>
	单标签： <标签名/>
		eg:<br />   <!--换行-->
4.标签关系			

	嵌套关系(父子关系) - 缩进一个tab位置
		<head>
			<title>title标签</title>
		</head>
	并列关系	
		<head>...</head>
		<body>...</body>
5.开发工具			

	DW  - dreamweaver
	S - Sublime  
	WebStorm	 
	HBuilder
	Visual Studio code 
6.doctype文档类型			
	
	<!DOCTYPE html>在html文档最前面，用于向用户和浏览器说明html的版本
7.字符集					
utf-8是目前最常用的字符集编码方式，常用的字符集编码方式还有gbk和gb2312.
	
	utf-8包含全世界所有国家需要用到的字符.
8.标签语义化
	
	先确定语义的HTML，在选合适的CSS
9.HTML标签			
	
	标题标签:  h1 - h7 		
	段落标签:  p 段落(paragraph) 默认情况下会根据窗口大小自动换行
	水平标签:	hr 横线(horizontal)
	换行标签:  br 打断、换行(break)
	** div span标签: 没有语义，网页布局用的盒子 division 分割。span跨度，范围
	文本格式化标签: (XHTML-HTML5推荐使用前者，语义更强烈,后者只有使用，没有强调的意思)
		<strong>...</strong>或<b>...</b> 粗体
		<em>...</em>或<i>...</i>斜体
		<del>...</del>或<s>...</s>加删除线
		<ins>...</ins>或<u>...</u>加下划线
10.标签属性		
语法格式:	&lt;标签名 属性1=“属性值1” 属性2=“属性值2”&gt;内容&lt;/标签名&gt;
	
	可以有多个属性，写在开始标签中，标签名后面；
	属性之间不分前后顺序，标签名语属性、属性语属性用空格分开；
	任何标签属性都有默认值，省略该属性则取默认值.
11.图像标签			
语法格式:&lt;img src="图像的URL"&gt;	

|属性|属性值|描述|
|---|---|---|
|src|URL|图像路径|
|alt|文本|图像不能显示时替换文本|
|title|文本|鼠标悬停时显示文本|
|width|像素，XHTML不支持页面百分比|设置图像宽度|		
|height|像素，XHTML不支持页面百分比|设置图像高度|
|border|数字|设置图像边框的宽度|
指定宽高的其中一个就行，另一个会根据图片尺寸等比例缩放。

12.链接标签anchor				
语法格式: &lt;a href="跳转目标" target="目标窗口弹出方式"&gt;文本或图像&lt;/a&gt;

	href：
		可以是外部链接，需添加http://www.baidu.com；
		可以是内部链接，<a href="index.html">首页</a>；
		如果未确定链接目标，通常将属性标签href属性值定义为"#",表示该链接暂时为一个空链接；
		网页中的各种网页元素(文本、图像、表格、音频、视频等)都可以添加超链接；
	target：用于指定链接页面的打开方式，默认self表示在本页面打开新的链接页面，blank值表示在新窗口中打开。
eg：

	<a href="http://www.baidu.com" target="blank">百度</a>
	target = "_blank"或“target”均可
13.锚点定位
通过定位的方式，可以快速的去向指定地方。		
eg：

	...
	早年经历<br />
	<a href="#movie">演绎经历</a><br />
	...
	
	<h3 id="movie">演绎经历</h3>
14.base标签 - 单标签		
语法格式: 在&lt;head&gt; &lt;base target="_blank"&gt; &lt;/&gt;		可以设置整体链接的打开状态。

```html

<head>
  <meta charset="UTF-8">
  <title>黑马视频 - Demo1</title>
  <link rel="stylesheet" href="html5-demo1.css">
  <base target="_blank">
</head>
<body>

  <a href="http://www.baidu.com" >百度</a>
  <a href="http://www.sina.com" >新浪</a>
  <a href="http://www.163.com" >网易</a>
  
  <script src="html5-demo1.js"></script>
</body>
```
15.特殊字符				
![demo2](demo2.png)				
16.注释标签			
语法格式:

	<!---这是一个注释--->
17.相对路径				
eg:图像文件(logo.gif)和html文件
	
	位于同一文件夹：如<img src=“logo.gif” />;
	图像位于html文件的下一级文件夹：如<img src=“img/img01/logo.gif” />;
	图像位于html文件的上一级文件夹：如<img src=“../logo.gif” />;
		如果是上两级，则需要使用"../../",一次类推。
18.列表				
![demo3](demo3.png)		
	
	无序列表	
		eg:
		<ul>
			<li>香蕉</li>
			<li>苹果</li>
			<li>荔枝</li>
		</ul>
	有序列表
		eg:
		<ol>
			<li>打开safari浏览器</li>
			<li>网页地址输入：百度</li>
			<li>键盘键入enter</li>
		</ol>
	自定义列表
		eg:
		<dl>
			<dt>名词1</dt>
			<dd>名词1解释1</dd>
			<dd>名词1解释2</dd>
			<dd>名词1解释3</dd>
			<dt>名词2</dt>
			<dd>名词2解释1</dd>
			<dd>名词2解释2</dd>
			<dt>名词3</dt>
			<dd>名词3解释1</dd>
			<dd>名词3解释2</dd>
			<dd>名词3解释3</dd>
		</dl>		

参考资料:		
1.黑马视频 - 入门基础
