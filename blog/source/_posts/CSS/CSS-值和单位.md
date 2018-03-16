---
title: CSS-值和单位
date: 2018-03-16 10:50:56
tags:	CSS
---

一、CSS的值

1.数值: 长度值，用于指定例如元素宽度、边框（border）宽度或字体大小；以及无单位整数。用于指定例如相对线宽或运行动画的次数。

1.1长度和尺寸:[lenght](https://developer.mozilla.org/zh-CN/docs/Web/CSS/length)

	<p>This is a paragraph.</p>
	<p>This is a paragraph.</p>
	<p>This is a paragraph.</p>
	
	CSS
	p {
	  margin: 5px;
	  padding: 10px;
	  border: 2px solid black;
	  background-color: cyan;
	}
	
	p:nth-child(1) {
	  width: 150px;
	  font-size: 18px;
	}
	
	p:nth-child(2) {
	  width: 250px;
	  font-size: 24px;
	}
	
	p:nth-child(3) {
	  width: 350px;
	  font-size: 30px;
	}
	
字号(font-size)代表字体/字形的高度;<br>
绝对单位:

	像素-px(pixels)
	mm, cm, in: 毫米（Millimeters），厘米（centimeters），英寸（inches = 2.54cm）
	pt, pc: 点（Points - 1pt=1/72in）， 十二点活字（ picas - 1pc=12pt）

	1in 总是（等于）96px
	3pt 总是（等于）4px
	25.4mm 总是（等于）96px

相对单位 - 相对于当前元素的字号（ font-size ）或者视口（viewport ）尺寸:	
	
	em:1em与当前元素的字体大小相同（更具体地说，一个大写字母M的宽度）。CSS样式被应用之前，浏览器给网页设置的默认基础字体大小是16像素，这意味着对一个元素来说1em的计算值默认为16像素。但是em单位是会继承父元素的字体大小，所以如果在父元素上设置了不同的字体大小，em的像素值就会变得复杂。em是Web开发中最常用的相对单位。
	ex, ch: 分别是小写x的高度和数字0的宽度。这些并不像em那样被普遍使用或很好地被支持;
	rem: REM（root em）和em以同样的方式工作，但它总是等于默认基础字体大小的尺寸；继承的字体大小将不起作用，这听起来像一个更好的选择比em，虽然在旧版本的IE上不被支持（查看关于跨浏览器支持 Debugging CSS.)
	vw, vh: 分别是视口宽度的1/100和视口高度的1/100，其次，它不像rem那样被广泛支持。

使用相对单位是非常有用的-调整HTML元素大小相对于字体或视口大小，这意味着布局将保持看起来正确.

1.2无单位的值
	
eg1:边框(内/外)
	
	一个元素完全去除外边框和内边框，你可以只使用无单位的0 —0就是0，不管前面设置的单位是什么！
	margin: 0

eg2:无单位的行高

	 line-height,设置元素中每行文本的高度。你可以使用单位设置特定的行的高度，但使用一个无量纲的值往往更容易，它就像一个简单的乘法因子。
	p {
		  line-height: 1.5;//这里默认字号16px;行高为1.5或24px（1.5x16）;
	}
eg3:动画数值-角度、时间、时长、次数
	
	<p>Hello</p>

	CSS
	@keyframes rotate {
	  0% {
	    transform: rotate(0deg);
	  }
	
	  100% {
	    transform: rotate(360deg);
	  }
	}
	
	p {
	  color: red;
	  width: 100px;
	  font-size: 40px;
	  transform-origin: center;
	}
	
	p:hover {
	  animation-name: rotate;
	  animation-duration: 0.6s;
	  animation-timing-function: linear;
	  animation-iteration-count: 5;
	}	
2.百分比: 可以用于指定尺寸或长度——例如取决于父容器的长度或高度，或默认的字体大小。

	<div>
	  <div class="boxes">Fixed width layout with pixels</div>
	  <div class="boxes">Liquid layout with percentages</div>
	</div>

	CSS
	div .boxes {
	  margin: 10px;
	  font-size: 200%;//字号为父容器的200%（默认字号16px）
	  color: white;
	  height: 150px;
	  border: 2px solid black;
	}
	
	.boxes:nth-child(1) {//第一个框红色背景-固定宽高
	  background-color: red;
	  width: 650px;
	}
	
	.boxes:nth-child(2) {//第二个框蓝色背景-固定高，动态宽(视口宽度75%)
	  background-color: blue;
	  width: 75%;
	}

动态布局(流体布局)--随浏览器视口大小的变化;
固定宽度布局--不管怎样都保持不变;
	
	可以使用动态布局来确保标准文档始终适合于屏幕，并且可以在不同的移动设备屏幕大小上表现良好。
	一个固定宽度的布局可以用来保持在线地图的大小相同；浏览器视口可以在地图上滚动，只在一个时间内查看一定数量的地图。您可以立即看到的量取决于您的视口有多大。
3.颜色: 用于指定背景颜色，字体颜色等。

颜色的描述方式:
	
	使用关键字
		red/green/yellow...
		eg:	background-color: red;
		
	使用RGB立体坐标（RGB cubic-coordinate）系统（以“#”加十六进制或者 rgb() 和 rgba() 函数表达式的形式）；
		eg:background-color: #ff0000;
		   background-color: rgb(255,0,0);
			background-color: rgba(255,0,0,0.5);
	使用HSL圆柱坐标（HSL cylindrical-coordinate）系统（以 hsl() 和 hsla() 函数表达式的形式）-- 包含r、g、b、色相、饱和度、明度值；
		色调：颜色的底色调。这个值在0到360之间，表示色轮周围的角度。
		饱和度：饱和度是多少？这需要一个价值从0-100%，其中0是没有颜色（它将显示为灰色），100%是全彩色饱和度。
		明度：颜色有多亮或明亮？这需要一个价值从0-100%，其中0是无光（它会出现全黑的），100%是充满光的（它会出现全白）
		eg:background-color: hsl(240,100%,50%);//蓝
			background-color: hsla(240,100%,50%,0.5);

通过CSS——opacity 属性设置不透明度和使用alpha通道设置不透明度的区别：
	
	alpha通道：0是完全透明的,1是完全不透明的；
	opacity属性：一切元素都是opacity的透明度；
	
	例如,当您想创建一个图片覆盖的标题，图片能透过标题显示，且标题的文本显示不受影响，此时应该使用RGBA修改标题背景色的透明度；另一方面,当您想要创建一个动画效果,整个UI元素从完全隐藏到可见，此时应该使用不透明度（Opacity）。


4.坐标位置: 例如，以屏幕的左上角为坐标原点定位元素的位置。


5.函数: 例如，用于指定背景图片或背景图片渐变。
	
	函数通常与JavaScript，Python，C++语言相关联，但它也作为属性值存在于CSS中。
	eg:
	background-color: hsla(240,100%,50%,0.5);
	transform: translate(50px, 60px);
	background-image: url('myimage.png');
参考资料：

1.[CSS手册](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Reference)

2.[CSS动画](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Animations)

3.[颜色描述](https://developer.mozilla.org/zh-CN/docs/Web/CSS/color_value#Color_keywords)




