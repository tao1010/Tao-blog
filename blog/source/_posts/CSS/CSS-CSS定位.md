---
title: CSS-CSS定位
date: 2018-04-10 16:33:49
tags: CSS
categories: Web
---

定位允许从正常的文档流布局中取出元素，并使他们具有不同的行为

一、文档流

1.盒模型：

	环绕元素内容添加任何内边距、边界和外边距来布置单个元素盒子。

2.块级元素：

	默认情况下，内容宽度是其父元素的宽度的100％，并且与其内容一样高；
	在视口中垂直布局——每个都将显示在上一个元素下面的新行上，并且它们的外边距将分隔开它们；
	
3.内联元素：

	高宽与他们的内容高宽一样。不能对内联元素设置宽度或高度——它们只是位于块级元素的内容中。 
	如果要以这种方式控制内联元素的大小，则需要将其设置为类似块级元素 display: block;
	不会出现在新行上；相反，它们互相之间以及任何相邻（或被包裹）的文本内容位于同一行上，只要在父块级元素的宽度内有空间可以这样做。如果没有空间，那么溢流的文本或元素将向下移动到新行；
	
4.外边距折叠：

	如果两个相邻元素都在其上设置外边距，并且两个外边距接触，则两个外边距中的较大者保留，较小的一个消失；

```html
<body>
  <h1>Basic document flow</h1>

  <p>I am a basic block level element. My adjacent block level elements sit on new lines below me.</p>
  
  <p>By default we span 100% of the width of our parent element, and we are as tall as our child content. Our total width and height is our content + padding + border width/height.</p>
  
  <p>We are separated by our margins. Because of margin collapsing, we are separated by the width of one of our margins, not both.</p>
  
  <p>inline elements <span>like this one</span> and <span>this one</span> sit on the same line as one another, and adjacent text nodes, if there is space on the same line. Overflowing inline elements will <span>wrap onto a new line if possible (like this one containing text)</span>, or just go on to a new line if not, much like this image will do: <img src="https://mdn.mozillademos.org/files/13360/long.jpg"></p>
</body>
```

```css
body {
  width: 500px;
  margin: 0 auto;
}

p {
  background: aqua;
  border: 3px solid blue;
  padding: 10px;
  margin: 10px;
}

span {
  background: red;
  border: 1px solid black;
}

```
![文档流](float-position.png)   
二、介绍定位  
1.静态定位


2.相对定位

3.介绍top,bottom,left,right

4.绝对定位

5.定位上下文

6.介绍z-index


7.固定定位


8.实验属性:position sticky


参考资料：   
1.[CSS定位](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/CSS_layout/定位)
