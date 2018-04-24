---
title: HTML基础知识-Head
date: 2018-03-08 11:57:23
tags: html
categories: Web
---

[Head中有什么？HTML中的元数据](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Introduction_to_HTML/The_head_metadata_in_HTML)

在页面加载完成的时候，标签head里的内容，是不会在页面中显示出来的。它的作用是包含一些页面的元数据:页面的&lt;title&gt;(标题) ,CSS(如果你想用CSS来美化页面内容)，图标和其他的元数据(比如 作者，关键词，摘要)。

1.增加一个标题

``` html
<head>
    <title>My test page</title>
</head>

<body>
    <h1>Mozilla is cool</h1>
</body>  
```

	当被加载到浏览器中的时候，元素 <h1>  会出现在页面中 —— 通常它应该在一个页面中只被使用一次, 它被用来标记你的页面内容的标题(故事的标题，新闻标题或者任何适当的方式）
	
	元素 <title> 是用来表示整个HTML文档大致内容的元数据(不是文档的内容.),也被以其他的方式使用着. 比如说，如果你尝试为某个页面添加书签，(Bookmarks > Bookmark This Page, 在火狐浏览器中), 你会看到 <title> 的内容被作为建议的书签名.

2.元数据：&lt;meta&gt;元素

元数据就是描述数据的数据，而HTML有一个“官方的”方式来为一个文档添加元数据，——  <meta> 元素. 当然，其他在这篇文章中提到的东西也可以被当作元数据。 有很多不同种类的 <meta> 元素可以被包含进你的页面的<head>元素。

eg1：指定文档中字符的编码

```html
<meta charset="utf-8">
```

	utf-8 是一个通用的字符集，它包含了任何人类语言中的大部分的字符。 这意味着你的web页面可以显示任意的语言; 所以对于你的每一个页面，使用这个设置是很好的! 比如说，你的页面可以很好的处理英语和日语.

eg2:添加作者和描述

``` html
<meta name="author" content="Chris Mills">
```
	name 特性指定了meta 元素的类型; 说明该元素包含了什么类型的信息。
	content 指定了实际的元数据内容。
	
	指定作者在某些情况下是很有用的：如果你需要联系页面的作者，问一些关于页面内容的问题。 一些内容管理系统能够自动获取页面作者的信息，然后用于某种目的。

	指定包含关于页面内容的关键字的页面内容的描述是很有用的，因为它可能或让你的页面在搜索引擎的相关的搜索出现得更多 (这些行为术语上被称为 Search Engine Optimization, or SEO.)

3.在站点增加自定义图标

16 x 16 像素是这种图标的第一种类型;这些图标出现在浏览器每一个打开的页面中的标签页中中以及在书签面板中的书签页面中。页面添加图标的方式:

	1.将其保存在与网站的索引页面相同的目录中，以.ico格式保存（大多数浏览器将支持更通用的格式，如.gif或.png，但使用ICO格式将确保它能在如Internet Explorer 6一样久远的浏览器显示）

	2.将以下行添加到HTML <head>中以引用它:
	<link rel="shortcut icon" href="favicon.ico" type="image/x-icon">
	
4.在HTML中应用CSS和JavaScript

几乎你使用的所有网站都会使用 CSS 让网页更加炫酷, 使用JavaScript让网页有交互功能, 比如视频播放器，地图，游戏以及更多功能。这些应用在网页中很常见，它们分别使用 &lt;link&gt;元素以及 &lt;script&gt;元素。
	
&lt;link&gt; 元素经常位于文档的头部，它有2个属性， rel="stylesheet"，表明这是文档的样式表，而 href,包含了样式表文件的路径：

``` html
<head>
...
<link rel="stylesheet" href="my-css-file.css">
...
</head>
```

 &lt;script&gt; 部分没必要非要放在文档头部; 实际上，把它放在文档的尾部（在 </body>标签之前）是一个更好的选择 ，这样可以确保在加载脚本之前浏览器已经解析了HTML内容（如果脚本加载某个不存在的元素，浏览器会报错）。

``` html	
<body>
...
<script src="my-js-file.js"></script>
</body>
```
tips：&lt;script&gt;元素看起来像一个空元素，但它并不是，因此需要一个结束标记。您还可以选择将脚本放入< script >元素中，而不是指向外部脚本文件


5.为文档设定主语言

为你的站点设定语言， 这个可以通过添加lang属性到HTML开始标签中来实现 (参考 [meta-example.html](https://github.com/mdn/learning-area/blob/master/html/introduction-to-html/the-html-head/meta-example.html))，如下所示：

``` html
<html lang="en-US">
```

还可以将文档的分段设置为不同的语言。例如， 我们可以把日语部分设置为日语， 如下所示：

``` html
<p>Japanese example: <span lang="jp">ご飯が熱い。</span>.</p>
```


参考资料：

1.[Head包含的元数据](https://developer.mozilla.org/en-US/docs/Learn/Discover_browser_developer_tools)