---
title: css基础
date: 2018-03-06 13:47:56
tags: css
---

一、CSS(层叠样式表)简介

css不是真正的编程语言，它是样式语言，允许你有选择地为HTML文档的元素添加样式。


二、CSS使用

在html文件中，head之中加入下列代码：

``` html
<link rel="stylesheet" href="styles/style.css" type="text/css">
```
保存html文件，浏览器打开即可生效

三、解析CSS

![解析CSS](css-declaration-small.png)

整个结构称为 CSS 的规则，注意里面单独的部分也是一样：

1.选择符（Selector）

HTML 元素放在规则最开始。它选择了一个或多个需要添加样式的元素（在这个例子中就是 p 元素）。要给不同元素添加样式只需要改变选择符就行了。

2.声明（Declaration）

一个单独的规则比如 color: red; 这指定了你想要添加样式元素的属性。

3.属性（Properties）

这是你改变 HTML 元素样式的方法。（在这个例子中 color 就是 p 元素的属性。）在 CSS 中，你通过选择合适的属性来改变你的规则。

4.属性值（Property value）

在属性的右边，冒号后面，我们拥有属性值，用于从指定的属性里的非常多的外观中选择一个值（我们除了 red 之外还有很多颜色的值可以选择）。

注意其他重要的语法：

	规则里除了选择器的部分都应该包含在成对的大括号里({}).。
	在声明里，你应该用冒号分离开属性与属性值。
	在规则里，你应该用分号分离开各个声明。
	如果要同时修改多个属性，你只需要将它们用分号隔开，就像这样：


eg1:修改多个属性：

```html
p{
	color:red;
	width:500px;
	border:1px solod black;
}
```
eg2:选择多个元素：

``` html
	p,li,h1{
		color:red;
	}
```

四、字体和文本

（ font-family 意味着你想要你的文本使用的字体。）这条规则首先为整个页面设定了一个全局字体和字体尺寸（因为 <html> 是整个页面的父元素，而且它所有的子元素都会继承相同的 font-size 和 font-family）

``` html

html {
    font-size: 10px; /* px 意为“像素（pixels）”: 这个基础字体的尺寸现在是10像素  */
    font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
  }
  
```

五、盒模型

写 CSS 时会发现大部分都是关于盒模型的——设置它们的尺寸，颜色，位置等等。大部分 HTML 元素都可以被看作一个一个层叠的盒子。CSS 布局就是基于盒模型的。每个占据你页面空间的块级元素都有相似的属性：

	内边距（padding）, 围绕着内容的空间（比如围绕段落的空间）
	边框（border）, 紧接着内边距的实体线段
	外边距(margin), 围绕元素外部的空间

	width (属于一个元素的)
	background-color, 元素内容和内边距之后的颜色
	color, 元素内容的颜色（通常是文本）
	text-shadow: 为元素内的文本设置阴影
	display: 设置元素的显示模式（暂时不用担心这部分内容）

eg1:优化布局

``` html
body{

    width: 600px;/* 强制页面永远保持600像素宽*/
    margin: 0 auto; /*当你在 margin 或 padding 中设置两个值时，第一个值代表元素的上方和下方（在这个例子中设置为 0），而第二个值代表左边和右边（在这里，auto 是一个特殊的值，意思是水平方向上左右对称）。你也可以使用一个，三个或四个值，查看 */
    background-color: #ff9500;/*指定元素的背景颜色*/
    padding: 0 20px 20px 20px;/*我们给内边距设置了四个值来让内容四周产生一点空间。这一次我们不设置上方的内边距，设置右边，下方，左边的内边距为20像素。值以上、右、下、左的顺序排列*/
    border: 5px solid black;/*简单地为页面四周设置了5像素的黑色实线边框*/
  }

```
[margin更多设置看这里](https://developer.mozilla.org/en-US/docs/Web/CSS/margin#Values)


eg2:阴影设置

``` html
 h1 {
    font-size: 60px;
    text-align: center;
    margin: 0;
    padding: 20px 0;    
    color: #00539F;
    text-shadow: 3px 3px 1px black;
  } 
```
text-shadow ，它为元素内容的字体提供了阴影。它的四个值如下：

	第一个像素值设置了水平方向的阴影值
	第二个像素值设置了垂直方向的阴影值
	第三个像素值设置了阴影模糊的距离（越大的值表示越模糊）
	第四个值设置了阴影的颜色

eg3: 图像居中

``` html
img {
	display: block;
	margin: 0 auto;
}	
```
最后，我们将使图像居中来让它变得更美观。我们可以重新使用 margin: 0 auto 一次，但是我们还要做其他调整。body 元素是块级元素，意味着它占据了页面的空间并且能够赋予外边距和其他改变间距的值。与之对应的是行内元素，则不能。所以为了使图像有外边距，我们必须使用 display: block 将其改成块级元素



参考资料：

1.[CSS基础](https://developer.mozilla.org/zh-CN/docs/Learn/Getting_started_with_the_web/CSS_basics)






