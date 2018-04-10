---
title: CSS-盒模型
date: 2018-03-17 11:25:37
tags: CSS 
toc: true
reward: true
categories: Web

---

一、简介

1.CSS盒模型(框模型）是网页布局的基础 ——每个元素被表示为一个矩形的方框，框的内容、内边距、边界和外边距像洋葱的膜那样，一层包着一层构建起来。

浏览器渲染网页布局时，它会算出每个框的内容要用什么样式，周围的洋葱层有多大，以及框相对于其它框放在哪里。

2.框属性

文档的每个元素被构造成文档布局内的一个矩形框，框每层的大小都可以使用一些特定的CSS属性调整。

![box-model-standard-small](box-model-standard-small.png)

	• width和height:设置内容框（content box）的宽度和高度。内容框是框内容显示的区域——包括框内的文本内容，以及表示嵌套子元素的其它框( 还有其他属性可以更巧妙地处理内容的大小——设置大小约束而不是绝对的大小。这些属性包括min-width、max-width、min-height 和 max-height。);
	• padding:表示一个 CSS 框的内边距 ——这一层位于内容框的外边缘与边界的内边缘之间。该层的大小可以通过简写属性padding 一次设置所有四个边，或用 padding-top、padding-right、padding-bottom 和 padding-left 属性一次设置一个边;（顺时针：上、右、下、左）
	• border:CSS 框的边界（border）是一个分隔层，位于内边距的外边缘以及外边距的内边缘之间。边界的默认大小为0——从而让它不可见——不过我们可以设置边界的厚度、风格和颜色让它出现。 border 简写属性可以让我们一次设置所有四个边，例如  border: 1px solid black; 但这个简写可以被各种普通书写的更详细的属性所覆盖：
		border-top, border-right, border-bottom, border-left: 分别设置某一边的边界厚度／风格／颜色。
		border-width, border-style, border-color: 分别仅设置边界的厚度／风格／颜色，并应用到全部四边边界。
		你也可以单独设置某一个边的三个不同属性，如 border-top-width, border-top-style, border-top-color，等。 
	•margin:外边距（margin）代表 CSS 框周围的外部区域，称为外边距，它在布局中推开其它 CSS 框。其表现与 padding 很相似；简写属性为 margin，单个属性分别为 margin-top、margin-right、margin-bottom 和 margin-left。

<font color=red>tips:</font> 外边距有一个特别的行为被称作外边距塌陷（margin collapsing）：当两个框彼此接触时，它们的间距将取两个相邻外边界的最大值，而非两者的总和.


二、玩转框

1.一些提示及想法:

	默认情况下background-color/background-image 延伸到了边界的边沿（the edge of the border）。该行为可以使用将在Background_clip 章中学到的background-clip 属性来改变。
	如果内容框变得比示例输出的窗口大，它将从窗口溢流，然后滚动条将会出现允许你滚动窗口来查看盒子剩余的内容 。你可以使用overflow属性来控制溢流——参看下边的 Overflow 章节。
	框的高度不监视百分比的长度；框的高度总是采用框内容的高度，除非指定一个绝对的高度（如：px 或者em），它会比在页面上默认是100%高度更实用。
	边界也忽略百分比宽度设置。
	框的总宽度(属性之和) = width + padding-right + padding-left + border-right + border-left。
	在一些情况下比较恼人（例如，假使你想要一个框占窗口宽度的50%，但边界和内边距是用像素来表示时该怎么办？），为了避免这种问题，有可能使用属性box-sizing来调整框模型。使用值border-box，它将框模型更改成这个新的模型：

![box-model-alt-small](box-model-alt-small.png)

三、高级框操作

在设置框的width, height, border, padding 及margin之外， 还有一些其他的属性可以改变它们的行为。

1.溢流

当使用绝对的值设置了一个框的大小（如，固定像素的宽/高），允许的大小可能不适合放置内容，这种情况下内容会从盒子溢流。我们使用[overflow](https://developer.mozilla.org/zh-CN/docs/Web/CSS/overflow)属性来控制这种情况的发生。它有一些可能的值，但是最常用的是：

	auto: 当内容过多，溢流的内容被隐藏，然后出现滚动条来让我们滚动查看所有的内容。
	hidden: 当内容过多，溢流的内容被隐藏。
	visible: 当内容过多，溢流的内容被显示在盒子的外边（这个是默认的行为）
	
eg:

	<div>
      <p class="autoscroll">
          Lorem ipsum dolor sit amet, consectetur adipiscing elit.
          Mauris tempus turpis id ante mollis dignissim. Nam sed
          dolor non tortor lacinia lobortis id dapibus nunc. Praesent
          iaculis tincidunt augue. Integer efficitur sem eget risus
          cursus, ornare venenatis augue hendrerit. Praesent non elit
          metus. Morbi vel sodales ligula.
       </p>
       
       <p class="clipped">
          Lorem ipsum dolor sit amet, consectetur adipiscing elit.
          Mauris tempus turpis id ante mollis dignissim. Nam sed
          dolor non tortor lacinia lobortis id dapibus nunc. Praesent
          iaculis tincidunt augue. Integer efficitur sem eget risus
          cursus, ornare venenatis augue hendrerit. Praesent non elit
          metus. Morbi vel sodales ligula.
       </p>
       
       <p class="default">
          Lorem ipsum dolor sit amet, consectetur adipiscing elit.
          Mauris tempus turpis id ante mollis dignissim. Nam sed
          dolor non tortor lacinia lobortis id dapibus nunc. Praesent
          iaculis tincidunt augue. Integer efficitur sem eget risus
          cursus, ornare venenatis augue hendrerit. Praesent non elit
          metus. Morbi vel sodales ligula.
       </p>
	  </div>
	  
	  CSS
	  /* 溢流 */
	p {
	  width  : 400px;
	  height : 2.5em;
	  padding: 1em 1em 1em 1em;
	  border : 1px solid black;
	}
	
	.autoscroll { overflow: auto;    }
	.clipped    { overflow: hidden;  }
	.default    { overflow: visible; }

![overflow](overflow.png)

2.背景裁剪(Background clip)

框的背景是由颜色和图片组成的，它们堆叠在一起（background-color, background-image）。 它们被应用到一个盒子里，然后被画在盒子的下面。默认情况下，背景延伸到了边界外沿。这通常是OK的，但是在一些情况下比较讨厌（假使你有一个平铺的背景图，你只想要它延伸到内容的边沿会怎么做？），该行为可以通过设置盒子的background-clip属性来调整。

eg:

	<div class="default"></div>
    <div class="padding-box"></div>
    <div class="content-box"></div>

	CSS
	/* 背景裁剪 */
	
	div {
	  width  : 60px;
	  height : 60px;
	  border : 20px solid rgba(0, 0, 0, 0.5);
	  padding: 20px;
	  margin : 20px 0;
	
	  background-size    : 140px;
	  background-position: center;
	  background-image   : url('https://mdn.mozillademos.org/files/11947/ff-logo.png');
	  background-color   : gold;
	}
	
	.default     { background-clip: border-box;  }
	.padding-box { background-clip: padding-box; }
	.content-box { background-clip: content-box; }
![Background-clip](Background-clip.png)

3.轮廓(Outline)

一个框的 outline 是一个看起来像是边界但又不属于框模型的东西。它的行为和边界差不多，但是并不改变框的尺寸（更准确的说，轮廓被勾画于在框边界之外，外边距区域之内）;
		
<font color=red>tips:</font>使用 outline 属性的时候要注意，它一般只在需要可用性的一些情况才被使用，例如在一些用户点击它的时候使用 outline 来表示高亮、可用，如果你要使用 outline，请确保不要因为它看起来像链接的高亮让用户迷惑。

	eg:
	
	HTML
	<p>hell world</p>
	 
	 CSS
	 /* 宽度 | 样式 | 颜色 */
	 p{
	 	outline: 1px solid red;
	 } 

四、CSS框类型

所有对于框的应用都表示的是块级元素的，然而，CSS还有一些表现不同的其他框类型。框类型应用通过指定display属性，这个属性有很多的属性值，三个最普遍的类型：block, inline, and inline-block。

	块框（ block box）是定义为堆放在其他框上的框（例如：其内容会独占一行），而且可以设置它的宽高，之前所有对于框模型的应用适用于块框 （ block box）
	行内框（ inline box）与块框是相反的，它随着文档的文字（例如：它将会和周围的文字和其他行内元素出现在同一行，而且它的内容会像一段中的文字一样随着文字部分的流动而打乱），对行内盒设置宽高无效，设置padding, margin 和 border都会更新周围文字的位置，但是对于周围的的块框（ block box）不会有影响。
	行内块状框（inline-block box） 像是上述两种的集合：它不会重新另起一行但会像行内框（ inline box）一样随着周围文字而流动，而且他能够设置宽高，并且像块框一样保持了其块特性的完整性，它不会在段落行中断开。（在下面的示例中，行内块状框会放在第二行文本上，因为第一行没有足够的空间，并且不会突破两行。然而，如果没有足够的空间，行内框会在多条线上断裂，而它会失去一个框的形状。）
	
<font color=red>tips:</font>默认状态下display属性值，块级元素display: block ，行内元素display: inline;
	
eg:

	<p>
	   Lorem ipsum dolor sit amet, consectetur adipiscing elit.
	   <span class="inline">Mauris tempus turpis id ante mollis dignissim.</span>
	   Nam sed dolor non tortor lacinia lobortis id dapibus nunc.
	</p>
	
	<p>
	  Lorem ipsum dolor sit amet, consectetur adipiscing elit.
	  <span class="block">Mauris tempus turpis id ante mollis dignissim.</span>
	  Nam sed dolor non tortor lacinia lobortis id dapibus nunc.
	</p>
	
	<p>
	  Lorem ipsum dolor sit amet, consectetur adipiscing elit.
	  <span class="inline-block">Mauris tempus turpis id ante mollis dignissim.</span>
	  Nam sed dolor non tortor lacinia lobortis id dapibus nunc.
	</p>

	CSS
	p {
	  padding : 1em;
	  border  : 1px solid black;
	}
	
	span {
	  padding : 0.5em;
	  border  : 1px solid green;
	
	  /* That makes the box visible, regardless of its type */
	  background-color: yellow;
	}
	
	.inline       { display: inline;       }
	.block        { display: block;        }
	.inline-block { display: inline-block; }
	
![block](block.png)	
	

参考资料：

1.[盒模型](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Introduction_to_CSS/Box_model)

2.[溢流](https://developer.mozilla.org/zh-CN/docs/Web/CSS/overflow)

3.[背景裁剪](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-clip)

4.[Outline](https://developer.mozilla.org/zh-CN/docs/Web/CSS/outline)