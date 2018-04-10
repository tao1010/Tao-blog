---
title: Html概述
date: 2018-03-05 16:10:13
tags: html
categories: Web
---

一、HTML简介

HTML(Hypertext Markup Language)并不是真正的编程语言，是一种用来结构化Web网页及其内容的标记语言。
HTML由一系列的元素所组成，这些元素可以用来封装不同部分的内容，使其以某种方式呈现或者工作。

二、元素

``` html
<p> My cat is very grumpy</p>
```

![解析元素](grumpy-cat-small.png)

<p id="hell_world_1">1.解析HTML元素：</p>

	开始标签（The opening tag）：这里包含了元素的名称（本例为 p），被开、闭尖括号所包围。这表示元素从此开始或者开始起作用——在本例中即段落由此开始。

	闭合标签（The closing tag）：与开始标签相似，只是其在元素名之前包含了一个斜杠。这表示着元素的结尾——在本例中，就是这一段落的结尾。

	内容（The content）：这是一个元素的内容，这个例子中就是所输入的文本本身。

	元素（The element）：开标签、闭标签与内容相结合，便是一个完整的元素。

![解析属性](grumpy-cat-attribute-small.png)

2.解析属性：

属性（Attribute）包含了关于元素的一些额外的信息，这些信息本身并不需要被显现在内容（Content）中。在这个例子中，class 是一个属性名称（name），editor-note 是属性的值(value) 。class 属性允许你为元素提供一个标识名称，以便进一步为元素指定样式或进行其他操作时使用。

一个属性应该包含：

	在属性与元素名称或上一个属性（如果有超过一个属性的话）之间的空格符
	
	属性的名称，并接上一个等号

	由引号所包围的属性值

<p id="hell_world_2">3.嵌套元素</p>

``` html
<p>My cat is <strong>very</strong>grumpy.</p>
```
将一个元素置于其他元素之中——这被称作嵌套.
ps:必须保证你的元素被正确地嵌套.

<p id="hell_world_3">4.空元素</p>

``` html
<img src="images/firefox-icon.png" alt= "My test image">
```

这里并没有 </img> 闭合标签，也没有内部内容。因为 image 元素不包含内容是有效果的，它的作用是向其所在的位置嵌入一个图像。

包括 alt (alternative) 属性 —— 这个属性应该是图像的描述内容，当图像不能被某些用户看见时，可能是因为：他们是盲人或者有视觉障碍。某些有严重视觉损伤的用户经常使用屏幕阅读器来为他们读出 alt 属性里的内容。

有些代码里的错误让图像不能被展示出来。举个例子，故意将 src 属性里的路径改错。如果你保存并且重新加载页面，你应该能在图像的位置看到"alt"对应的内容；

alt 属性的关键就是要“可以描述图像的文本”。当被读出来时， alt 里面的内容应该向用户传递足够图像表达的意思。

三、标记文本

1.标题

HTML包括六个级别的标题,h1-h6,虽然你可能只会用到3-4最多.

``` HTML
<h1>My main title</h1>

<h2>My top level heading</h2>

<h3>My subheading</h3>

<h4>My sub-subheading</h4>

```

2.段落

``` HTML
<p>This is a single paragraph</p>

```

3.列表

最常用的列表类型是有序列表（ ordered lists）和无序列表（ unordered lists）：

	无序列表 中项目的顺序并不重要，就像购物列表。这些内容被包括在一个 <ul> 元素里。

	有序列表 中项目的顺序很重要，就像一个食谱。这些内容被包括在一个 <ol> 元素里。

列表内的每个项目被包括在一个 <li> (list item)元素里。

``` HTML

<p>At Mozilla, we’re a global community of</p>
        <ul> 
            <li>technologists</li>
            <li>thinkers</li>
            <li>builders</li>
        </ul>
        <ol>
            <li>technologists</li>
            <li>thinkers</li>
            <li>builders</li>
        </ol>

```

4.链接

文本添加链接的步骤：

	1.添加一些文本。我们选择“Mozilla Manifesto”。

	2.将文本包含在“尖括号a”元素内，就像这样：

``` HTML
	<a>Mozilla Manifesto</a>
```

	3.赋予 <a> 元素一个 href 属性，就像这样：

``` HTML
	<a href="">Mozilla Manifesto</a>
```
	4.把你想要链接的网址放到 href 属性内：

``` HTML
<a href="https://www.mozilla.org/en-US/about/manifesto/">Mozilla Manifesto</a>

```

ps:一些文件路径的通用规则：

    1.要引用一个位于同级目录的 HTML 文件，只需直接使用文件名，比如  my-image.jpg。
    2.要引用一下子目录的文件，在路径前写下目录名并加一个斜杠，比如 subdirectory/my-image.jpg。
    3.要引用一个父目录的 HTML 文件，加上两个点。举个例子，如果 index.html 在  test-site 子目录而 my-image.png 与  test-site 处于同级目录，你可以使用 ../my-image.png 在 index.html 内引用 my-image.png 。
    
你可以组合随意以上方法，比如 ../subdirectory/another-subdirectory/my-image.png.


参考资料：

1.[HTML教程](https://developer.mozilla.org/zh-CN/docs/Web/HTML)
