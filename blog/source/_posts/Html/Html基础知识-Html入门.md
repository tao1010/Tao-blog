---
title: HTML基础知识 - HTML入门
date: 2018-03-08 09:17:47
tags: html
---

[HTML入门](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Introduction_to_HTML/Getting_started)

1.[剖析一个HTML元素](https://tao1010.github.io/2018/03/05/Html概述/#hell_world_1)

2.斜体强调标签

``` html
	<em>这是一个斜体</em>
```

3.[嵌套元素](https://tao1010.github.io/2018/03/05/Html概述/#hell_world_2)

4.块级元素和内联元素

块级元素

	在页面中以块的形式展现 —— 相对与其前面的内容它会出现在新的一行，其后的内容也会被挤到下一行展现。
	
	块级元素通常用于展示页面上结构化的内容，例如段落、列表、导航菜单、页脚等等。
	
	一个以block形式展现的块级元素不会被嵌套进内联元素中，但可以嵌套在其它块级元素中。
	
eg：

``` html
<p>fourth</p><p>fifth</p><p>sixth</p>
```
	
内联元素
	
	通常出现在块级元素中并包裹文档内容的一小部分，而不是一整个段落或者一组内容。
	
	内联元素不会导致文本换行：它通常出现在一堆文字之间例如超链接元素<a>或者强调元素<em>和 <strong>。
	
eg:

``` html
<em>first</em><em>second</em><em>third</em>
```

5.[空元素](https://tao1010.github.io/2018/03/05/Html概述/#hell_world_3)

不是所有元素都拥有开始标签，内容和结束标记. 一些元素只有一个标签，通常用来在此元素所在位置插入/嵌入一些东西 。
	
	例如：元素<img>是用来在元素<img>所在位置插入一张指定的图片。

6.属性

一个属性必须包含如下内容：

	在元素和属性之间有个空格space (如果有一个或多个已存在的属性，就与前一个属性之间有一个空格.)
	
	属性后面紧跟着一个“=”符号.
	
	有一个属性值,由一对引号“ ”引起来. 
		- 否则会引起浏览器的误解析。
		- 所以属性可以是双引号"",也可以是单引号‘’,但不能混用，可以嵌套。

eg1：属性

``` html
<a href="https://www.mozilla.org/" title="The Mozilla homepage" target="_blank">这是一个超链接</a>	
```

	href: 这个属性声明超链接的web地址，当这个链接被点击浏览器会跳转至href声明的web地址。例如： href="https://www.mozilla.org/"。

	title: 标题title 属性为超链接声明额外的信息，比如你将链接至那个页面。例如： title="The Mozilla homepage"。当鼠标悬浮时，将出现一个工具提示。
	
	target: 目标target 属性指定将用于显示链接的浏览上下文。 例如， target="_blank" 将在新标签页中显示链接。如果你希望在目前标签页显示链接，只需忽略这个属性。 
	
eg2:属性引号

``` html
<a href="http://www.example.com">A link to my example.</a>
<a href='http://www.example.com'>A link to my example.</a>
<a href="http://www.example.com" title="Isn't this fun?">A link to my example.</a>

```
	
6.1布尔属性:

有时你会看到没有值的属性，它是合法的。这些属性被称为布尔属性，他们只能有跟它的属性名一样的属性值。

	例如 disabled 属性，他们可以标记表单输入使之变为不可用(变灰色)，此时用户不能向他们输入任何数据

``` html
<input type="text" disabled="disabled">	//输入框不可用
<input type="text" disabled>			//输入框不可用
<input type="text">						//输入框可用

```	
7.分析HTML文档

完整的HTML页面：

``` html
	<!DOCTYPE html>
	<html>
	  <head>
	    <meta charset="utf-8">
	    <title>My test page</title>
	  </head>
	  <body>
	    <p>This is my page</p>
	  </body>
	</html>
	
```

分析如下：
	
	1.<!DOCTYPE html>: 声明文档类型
	2.<html></html>: <html>元素。这个元素包裹了整个完整的页面，是一个根元素。	
	3.<head></head>: <head>元素. 这个元素是一个容器，它包含了所有你想包含在HTML页面中但不想在HTML页面中显示的内容。这些内容包括你想在搜索结果中出现的关键字和页面描述，CSS样式，字符集声明等等。
	4.<meta charset="utf-8">: 这个元素设置文档使用utf-8字符集编码，utf-8字符集包含了人类大部分的文字。基本上他能识别你放上去的所有文本内容。毫无疑问要使用它，并且它能在以后避免很多其他问题。
	5.<title></title>: 设置页面标题，出现在浏览器标签上，当你标记/收藏页面时它可用来描述页面。
	6.<body></body>: <body>元素。 包含了你访问页面时所有显示在页面上的内容，文本，图片，音频，游戏等等。
	
HTML中的空白

	无论你用了多少空白(包括空白字符，包括换行), 当渲染这些代码的时候，HTML解释器会将连续出现的空白字符减少为一个单独的空格符。
	那么为什么我们会使用那么多的空白呢? 答案就是为了可读性 —— 如果你的代码被很好地进行格式化，那么就很容易理解你的代码是怎么回事, 反之就只有聚做一团的混乱. 
	在我们的HTML代码中，我们让每一个嵌套的元素以两个空格缩进。 你使用什么风格来格式化你的代码取决于你 (比如所对于每层缩进使用多少个空格),但是你应该坚持使用某种风格。

8.实体引用：在HTML中包含特殊字符

在HTML中，字符 <, >,",' 和 & 是特殊字符. 我们必须使用字符引用 —— 表示字符的特殊编码, 它们可以在那些情况下使用. 每个字符引用以符号&开始, 以分号(;)结束.

![Html基础知识](Html-basic.png)

9.HTML注释

为了将一段HTML中的内容置为注释，你需要将其用特殊的记号<!--和-->包括起来。

``` html
<p>I'm not inside a comment</p>

<!-- <p>I am!</p> -->
```


参考资料：

1.[XML和HTML字符实体引用列表](http://en.wikipedia.org/wiki/List_of_XML_and_HTML_character_entity_references)



