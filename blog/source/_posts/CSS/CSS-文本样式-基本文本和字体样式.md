---
title: CSS-文本样式-基本文本和字体样式
date: 2018-03-18 10:54:56
tags: CSS

---

一、CSS文字样式

元素中的文本是布置在元素的内容框中。以内容区域的左上角作为起点 (或者是右上角，是在 RTL 语言的情况下)，一直延续到行的结束部分。

换行：&lt;br&gt;

用于样式文本的 CSS 属性通常可以分为两类:

	字体样式: 作用于字体的属性，会直接应用到文本中，比如使用哪种字体，字体的大小是怎样的，字体是粗体还是斜体，等等。
	
	文本布局风格: 作用于文本的间距以及其他布局功能的属性，比如，允许操纵行与字之间的空间，以及在内容框中，文本如何对齐。

<font color=red>注意:</font>包含在元素中的文本是作为一个单一的实体。不能将文字其中一部分选中或添加样式，如果要这么做，那么必须要用适合的元素来包装它们，比如：

	 1）<span> 或者 <strong>;
	 2）使用伪元素，像如下
		 ::first-letter (选中元素文本的第一个字母), 
		 ::first-line (选中元素文本的第一行), 
		 ::selection (当前光标双击选中的文本)
1.字体

1)颜色 ：color 属性设置选中元素的前景内容的颜色 (通常指文本，也包含一些其他东西，或者是使用 text-decoration 属性放置在文本下方或上方的线 (underline overline)。
	
	color:red; 
	color:rgb(255,255,255);rgba(255,255,255,1);
	color:#ffffff;

2)字体种类:浏览器只会把在当前机器上可用的字体应用到当前正在访问的网站上；如果字体不可用，那么就会用浏览器默认的字体代替 default font. 

	font-family:arial;//在文本上设置一个不同的字体;

网页安全字体：只有某几个字体通常可以应用到所有系统。

![safe-web-font](safe-web-font.png)

默认字体：CSS定义了5个常用的字体名称，使用的字体完全取决于每个浏览器和操作系统。

![default-font](default-font.png)

字体栈：由于无法保证在网页上使用的字体的可用性 (甚至一个网络字体可能由于某些原因而出错), 可以提供一个字体栈 (font stack):只需包含一个font-family属性，其值由几个用逗号分离的字体名称组成。

	p {
	  font-family: "Trebuchet MS", Verdana, sans-serif;
	}
	在这种情况下，浏览器从列表的第一个开始，然后查看在当前机器中，这个字体是否可用。如果可用，就把这个字体应用到选中的元素中。如果不可用，它就移到列表中的下一个字体，然后再检查。
tips:有一些字体名称不止一个单词，比如Trebuchet MS ，那么就需要用引号包裹。

	p {
	  color: red;
	  font-family: Helvetica, Arial, sans-serif;
	}

3)字体大小：font-size

常用单位:
	
	px:将像素的值赋予给文本,绝对单位;
	
	ems:1em等于设计的当前元素的父元素上设置的字体大小 (更加具体的话，比如包含在父元素中的大写字母 M 的宽度) ;
	如果有大量设置了不同字体大小的嵌套元素，这可能会变得棘手, 但它是可行的;
	可以使用em调整任何东西的大小，不只是文本。可以有一个单位全部都使用 em 的网站，这样维护起来会很简单。
	
	rems:和ems差不多，除了 1rem 等于 HTML 中的根元素的字体大小，而不是父元素。这可以让你更容易计算字体大小，但是遗憾的是，rems不支持IE8及以下的版本;

元素的 font-size 属性是从该元素的父元素继承的。所以这一切都是从整个文档的根元素——&lt;html&gt;开始，浏览器的 font-size 标准设置的值为 16px。在根元素中的任何段落 (或者那些浏览器没有设置默认大小的元素)，会有一个最终的大小值：16px。其他元素也许有默认的大小:

```html
比如 <h1> 元素有一个 2ems 的默认值，所以它的最终大小值为 32px。当你开始更改嵌套元素的字体大小时，事情会变得棘手。
比如，如果你有一个 <article> 元素在你的页面上，然后设置它的 font-size 为 1.5ems (通过计算，可以得到大小为 24px)，然后想让 <article> 元素中的段落获得一个计算值为 20px 的大小，那么你应该使用多少 em。
```
<font color=red>当调整你的文本大小时，将文档(document)的基础  font-size 设置为10px往往是个不错的主意，这样之后的计算会变得简单，所需要的 (r)em 值就是想得到的像素的值除以 10，而不是 16。</font>

	html {
	  font-size: 10px;
	}
	
	h1 {
	  font-size: 2.6rem;
	}
	
	p {
	  font-size: 1.4rem;
	  color: red;
	  font-family: Helvetica, Arial, sans-serif;
	}
4)字体样式，字体粗细、文本转换、文本装饰

	字体样式font-style：用来打开和关闭文本 italic (斜体)。
		• normal: 将文本设置为普通字体 (将存在的斜体关闭)
		• italic: 如果当前字体的斜体版本可用，那么文本设置为斜体版本；如果不可用，那么会利用 oblique 状态来模拟 italics。
		• oblique: 将文本设置为斜体字体的模拟版本，也就是将普通文本倾斜的样式应用到文本种。
		
	字体粗细font-weight:设置文字的粗体大小。多值可选(比如 -light, -normal, -bold, -extrabold, -black, 等等), 常用 normal 和 bold值：
		normal, bold: 普通或者加粗的字体粗细
		lighter, bolder: 将当前元素的粗体设置为比其父元素粗体更细或更粗一步。100–900: 数值粗体值，如果需要，可提供比上述关键字更精细的粒度控制。
		
	文本转换text-transform:允许你设置要转换的字体。值包括：
		none: 防止任何转型。
		uppercase: 将所有文本转为大写。
		lowercase: 将所有文本转为小写。
		capitalize: 转换所有单词让其首字母大写。
		full-width: 将所有字形转换成固定宽度的正方形，类似于等宽字体，允许对齐。拉丁字符以及亚洲语言字形（如中文，日文，韩文）
		
	文本装饰text-decoration:设置/取消字体上的文本装饰 (你将主要使用此方法在设置链接时取消设置链接上的默认下划线。) 可用值为：
		none: 取消已经存在的任何文本装饰。
		underline: 文本下划线.
		overline: 文本上划线
		line-through: 穿过文本的线 strikethrough over the text.
	text-decoration 可以一次接受多个值，如果你想要同时添加多个装饰值， 比如 text-decoration: underline overline.。
	text-decoration 是一个缩写形式，它由 text-decoration-line, text-decoration-style 和 text-decoration-color 构成。比如 text-decoration: line-through red wavy.
<span>
eg:

	html {
	  font-size: 10px;
	}
	
	h1 {
	  font-size: 2.6rem;
	  text-transform: capitalize;
	}
	
	h1 + p {
	  font-weight: bold;
	}
	
	p {
	  font-size: 1.4rem;
	  color: red;
	  font-family: Helvetica, Arial, sans-serif;
	}
5）文字阴影

语法：

	/* offset-x | offset-y | blur-radius | color */
	text-shadow: 1px 1px 2px black; 
	
	/* color | offset-x | offset-y | blur-radius */
	text-shadow: #CCC 1px 0 10px; 
	
	/* offset-x | offset-y | color */
	text-shadow: 5px 5px #558ABB;
	
	/* color | offset-x | offset-y */
	text-shadow: white 2px 5px;
	
	/* offset-x | offset-y
	/* Use defaults for color and blur-radius */
	text-shadow: 5px 10px;
	
	text-shadow: inherit;
	
属性(4个)如下:

	1	阴影与原始文本的水平偏移，可以使用大多数的 CSS 单位 length and size units, 但是 px 是比较合适的。这个值必须指定。
	2	阴影与原始文本的垂直偏移;效果基本上就像水平偏移，除了它向上/向下移动阴影，而不是左/右。这个值必须指定。
	3	模糊半径 - 更高的值意味着阴影分散得更广泛。如果不包含此值，则默认为0，这意味着没有模糊。可以使用大多数的 CSS 单位 length and size units.
	4	阴影的基础颜色，可以使用大多数的 CSS 颜色单位 CSS color unit. 如果没有指定，默认为 black.
	
	正偏移值可以向右移动阴影，但也可以使用负偏移值来左右移动阴影，例如 -1px -1px.
	
多种阴影：通过包含以逗号分隔的多个阴影值，将多个阴影应用于同一文本：
	
	text-shadow: -1px -1px 1px #aaa,
             0px 4px 1px rgba(0,0,0,0.5),
             4px 4px 5px rgba(0,0,0,0.7),
             0px 0px 7px rgba(0,0,0,0.4);
2.文本布局

1）文本对齐： 

text-align 属性用来控制文本如何和它所在的内容盒子对齐

	left: 左对齐文本。
	right: 右对齐文本。
	center: 居中文字
	justify: 使文本展开，改变单词之间的差距，使所有文本行的宽度相同。当应用于其中有很多长单词的段落时。如果你要使用这个，你也应该考虑一起使用别的东西，比如 hyphens，打破一些更长的词语。
2）行高:

line-height属性设置文本每行之间的高，可以接受大多数单位[length and size units](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Introduction_to_CSS/Values_and_units#Length_and_size)，不过也可以设置一个无单位的值，作为乘数，通常这种是比较好的做法。无单位的值乘以 font-size 来获得 line-height。当行与行之间拉开空间，正文文本通常看起来更好更容易阅读。推荐的行高大约是 1.5–2 (双倍间距。) 所以要把我们的文本行高设置为字体高度的1.5倍。

推荐使用：

	line-height: 1.5;
3）字母和字间距

letter-spacing 和 word-spacing 属性允许你设置你的文本中的字母与字母之间的间距、或是字与字之间的间距。
	
eg:段落的第一行
	
	p::first-line {
	  letter-spacing: 2px;
	  word-spacing: 4px;
	}
4）其他属性

Font样式
文本布局样式


3.Font简写

许多字体的属性也可以通过 font 的简写方式来设置 . 

这些是按照以下顺序来写的：  font-style, font-variant, font-weight, font-stretch, font-size, line-height, and font-family.

在所有这些属性中，只有 font-size 和 font-family 是一定要指定的，如果你想要 font 的简写形式。

一个正斜杠必须放在 font-size 和 line-height 属性之间。

	font: italic normal bold normal 3em/1.5 Helvetica, Arial, sans-serif;


参考资料：

1.[CSS学习路径](https://developer.mozilla.org/zh-CN/docs/Learn/CSS)

2.[为文本添加样式](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/为文本添加样式)

3.[基本文本和字体样式](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/为文本添加样式/Fundamentals)

4.[text-shadow阴影](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-shadow)

5.[hyphens](https://developer.mozilla.org/zh-CN/docs/Web/CSS/hyphens)