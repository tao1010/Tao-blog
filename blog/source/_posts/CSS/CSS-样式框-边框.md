---
title: CSS-样式框-边框
date: 2018-03-21 14:10:53
tags: CSS
categories: Web
---

一、边界回顾

1.边界简写
	
	border:10px solid red;
2.普通写法

border可以分解成许多不同的属性:

	border-top, border-right, border-bottom, border-left: 设置边界一侧的宽度，样式和颜色。
	border-width, border-style, border-color: 设置边界宽度度、样式或颜色，但是会设置边界的四个边。
	您还可以单独三个属性中的一个并且设置其中一侧边界生效。
	border-top-width, border-top-style, border-top-color等。
当没有明确设置值时，边界会默认使用文本的颜色，宽度为3px.

二、边界半径

1.不同的角落放置不同大小的边界半径, 可以指定两个，三个或四个值, 就像使用 padding and margin一样:
	
	p{
		border-radius: 20px;
		
		/* 1st value is top left and bottom right corners,
		   2nd value is top right and bottom left  */
		border-radius: 20px 10px;
		
		/* 1st value is top left corner, 2nd value is top right
		   and bottom left, 3rd value is bottom right  */
		border-radius: 20px 10px 50px;
		
		/* top left, top right, bottom right, bottom left */
		border-radius: 20px 10px 50px 0;
		
	}
2.还可以创建椭圆形角（x半径与y半径不同）。两个不同的半径用正斜杠（/）分隔，您可以将其与值的任意组合组合:

	p{
		border-radius: 10px / 20px;
		border-radius: 10px 30px / 20px 40px;
	}
	
可以使用任何长度单位来指定边界半径，例如 ： px, rems, %.

还可以使用普通写法属性，分别设置框的每个角的边界半径：
	
	border-top-left-radius,
	border-top-right-radius,
	border-bottom-left-radius,
    border-bottom-right-radius。

三、边界图像

border-image图像使实现复杂的图形边界变得容易得多，即使必须在现代浏览器中才能实现(IE11+支持它，以及其他现代浏览器).

1.原理

需要有一个映像应用到您的浏览器。这通常是3 x 3、4 x 4、5 x 5(等等)网格设计，如下所做的：
![border-image](border-image.png)

当这样的图像用于边界图像时，浏览器将图像分割为8个部分：
![border-slices](border-slices.png)

这些角的图像会被插入到你的边界的角落里，而顶部、右边、底部和左边的部分将被用来填充你的边界的相应边(通过拉伸或重复)。我们需要告诉浏览器让这些片的大小正确——例如，这个图像是160px，还有一个4 x 4的网格，所以每个片都需要40px.
	
1.1需要一个盒子来应用边界。这需要指定一个边界，否则边界图像将没有显示的空间。还将使用background-clip，使任何背景色只填充内容和内边距的区域，并且不扩展到边界。

		border: 30px solid black;
		background-clip: padding-box;
1.2使用 border-image-source指定要使用的源图像作为边界图像。 它的工作原理和background-image一样，能够接受一个url()函数或一个渐变作为一个值.
		
		border-image-source: url(border-image.png);
1.3将使用border-image-slice来设置所需大小的切片，如上所述：

		border-image-slice: 40;
<span>

	如果所有的片都是相同的大小，那么这个属性可以取一个值，如果需要不同的大小，则可以使用多个值，以相同的方式使用padding和margin：
		两个值:上和下，左和右。
		三个值:上、左和右、下。
		四个值:上、右、下、左。
	如果图像是光栅图形（像 .png 或 .jpg），就用像素来解释这个数字。如果图像是矢量图形（比如，.svg），那么这个数字将被解释为图形中的坐标。也可以使用百分比（使用单位 %)。
1.4将使用border-image-repeat t来指定我们希望图像如何填充边界。选项是：

	stretch：默认;侧面的图像被拉伸来填满边界。这通常看起来很糟糕和像素化，所以不推荐。
	repeat：边图像被重复，直到边界被填满。根据具体情况，这可能看起来不错，但您可能会看到一些难看的图像片段。
	round： 边的图像被重复，直到边界被填满，它们都被稍微拉伸，这样就不会出现碎片。
	space：边图像被重复，直到边界被填满，每个拷贝之间添加了少量的间隔，这样就不会出现任何片段。这个值只在Safari(9+)和Internet Explorer(11+)中得到支持。
决定使用round的值，因为它看起来是最有用和最灵活的：

	border-image-repeat: round;
	
	div {
	  width: 300px;
	  padding: 20px;
	  margin: 10px auto;
	  line-height: 3;
	  background-color: #f66;
	  text-align: center;
	  /* border-related properties */
	  border: 20px solid black;
	  background-clip: padding-box;
	  border-image-source: url(https://mdn.mozillademos.org/files/13060/border-image.png);
	  border-image-slice: 40;
	  border-image-repeat: round;
	}
![border-demo](border-demo.png)

2.其他属性

	border-image-width：只调整边界图像，而不是边界——如果这个设置小于border-width，它会在边界的外面，而不是填满它。如果它更大，那么它就会扩展到边界之外，并开始重叠在内边距/内容上。
	border-image-outset：定义边界内部和内边距之间的额外空间的大小——有点像“边界填充”。如果需要的话，这是一种简单的方法，可以将边界图像移出一点。
3.简写

	border-image-source: url(border-image.png);
	border-image-slice: 40;
	border-image-repeat: round;
等价于
	
	border-image: url(border-image.png) 40 round;

参考资料：

1.[CSS样式边框](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Styling_boxes/Borders)