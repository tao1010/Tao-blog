---
title: CSS-样式框-背景
date: 2018-03-20 11:19:49
tags: CSS
categories: Web
---

一、框模型(盒子模型)概要 - 回顾

1.盒子的属性
	
	width/height
	padding - 内边距(top/right/bottom/left) 
	border - 边界(宽度、样式、颜色)
	margin - 外边距(top/right/bottom/left)，外边距塌陷
2.[溢流-overflow](https://developer.mozilla.org/zh-CN/Learn/CSS/Introduction_to_CSS/Box_model#Overflow)

3.[背景裁剪 - background clip](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Getting_started/Boxes)

4.[轮廓 - outline](https://developer.mozilla.org/zh-CN/docs/Web/CSS/outline)

5.高级属性
	
	设置width和height的约束;
	完全改变盒模型;
6.盒子显示(display)类型
	
	常见的display类型:block/inline/inline-block
	不常见的display类型:table/flex/grid
二、背景

1.概述

元素的背景是指，在元素内容、内边距和边界下层的区域。默认情况下就是这样——在新的浏览器中，你可以使用 background-clip属性改变背景所占用的区域。

背景并不在外边距下层——外边距不是元素区域的一部分，而是元素外面的区域。

	background-color: 为背景设置一个纯色。
	background-image: 指定在元素的背景中出现的背景图像。
	这可以是静态文件，也可以是生成的渐变。
	background-position:指定背景应该出现在元素背景中的位置。
	background-repeat: 指定背景是否应该被重复(平铺)。
	background-attachment: 当内容滚动时，指定元素背景的行为，例如，它是滚动的内容，还是固定的?
	background: 在一行中指定以上五个属性的缩写。
	background-size: 允许动态调整背景图像的大小。
2.基本内容:
	
	背景颜色background-color
背景图像background-image 

	属性指定了在元素背景中显示的背景图像。该属性最简单的用法是使用 url() 函数——它以一个参数的路径作为参数——获取一个静态图像文件来插入;
		eg:background-image: url(https://xxx/xxx/xxx.png)
		
背景重复background-repeat

	允许您指定背景图像是如何重复的。默认值是repeat;
	
	background-repeat: no-repeat;	
	no-repeat: 图像将不会重复:它只会显示一次。
	repeat-x: 图像将在整个背景中水平地重复。
	repeat-y: 图像会在背景下垂直地重复。

背景位置background-position

	允许我们在背景中任意位置放置背景图像。通常，该属性将使用两个通过空格分隔的值，该空间指定了图像的水平(x)和垂直(y)坐标。图像的左上角是原点(0,0)。把背景想象成一个图形，x坐标从左到右，y坐标从上到下。
	该属性可以接受许多不同的值类型，最常用的是:
		•像px这样的绝对值——比如 background-position: 200px 25px.
		•像rems 这样的相对值——比如 background-position: 20rem 2.5rem.
		•百分比 ——比如 background-position: 90% 25%.
		•关键字——比如 background-position: right center. 这两个值是直观的，可以分别取值比如 left，center， right和 top，center， bottom。
	
3.背景图像：渐变

background-image:linear-gradient();

	linear-gradient()渐变函数
	函数至少需要用逗号分隔的三个参数——背景中渐变的方向，开始的颜色和结尾的颜色.
3.1线性渐变:（一条直线到一条直线）

	div {
		background-image: linear-gradient(to bottom, orange, yellow);
		//渐变将从上到下，从顶部的橙色开始，然后平稳过渡到底部的黄色
	}
<span>			

	//使用关键字来指定方向 （to bottom，to right， to bottom right等）， 或角度值 (0deg相当于 to top，90deg 相当于 to right，直到 360deg，它再次相当于 to top ）
	eg:linear-gradient(to bottom, yellow, orange 40%, yellow);
	
	//也可以在颜色定义的渐变中指定其他的点——这些被称为颜色站点(color stops)，浏览器会计算出每一组颜色站点之间的颜色渐变
	eg:repeating-linear-gradient(to right, yellow, orange 25px, yellow 50px);
3.2径向渐变：(从一个点发散出来)

4.背景附着background-attachment

	scroll将把背景修改为页面视图，因此它将在页面滚动时滚动。注意，我们说的是视图，而不是元素——如果你滚动实际的背景设置的元素，而不是页面，背景不会滚动
	fixed可以在页面的位置上固定背景，所以当页面滚动时，它不会滚动，不管你是滚动页面还是背景设置的元素，它都会保持在相同的位置
	local将背景设置为它所设置的元素的背景，因此当您滚动元素时，背景会随之滚动。
5.背景简写
		background: yellow linear-gradient(to bottom, orange, yellow) no-repeat left center scroll;		 
		 
6.多个背景:(图片+渐变同时显示)

IE9+已经具备了将多个背景连接到单个元素的能力:

	div {
		background: url(image.png) no-repeat 99% center,
		            url(background-tile.png),
		            linear-gradient(to bottom, yellow, #dddd00 50%, orange);
		background-color: yellow;
	}
	
	这些背景都是堆叠在一起的，第一个出现在顶部，第二个在下面，第三个，等等。这可能不是你所期待的，所以要小心。
也可以将多个值放入到普通写法的 background-*属性中:

	background-image: url(image.png), url(background-tile.png);
	background-repeat: no-repeat, repeat;
7.背景尺寸background-size:

允许你动态地改变背景图像的大小，使它更适合你的设计.

对于每个背景图像，您需要包含两个背景大小值，一个用于水平维度，另一个用于垂直方向：

	background-image: url(myimage.png);
	background-size: 16px 16px;
	
	可以使用你期望的所有长度单位来指定你想要的值——px，百分比， rem等.

参考资料：

1.[样式框](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Styling_boxes)

2.[样式框-背景](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Styling_boxes/背景)

3.[background-clip coverage](https://developer.mozilla.org/zh-CN/Learn/CSS/Introduction_to_CSS/Box_model#Background_clip)

4.[CSS3 Radial Gradients](https://dev.opera.com/articles/css3-radial-gradients/)