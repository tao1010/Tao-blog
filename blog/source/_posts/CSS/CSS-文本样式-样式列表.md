---
title: CSS-文本样式-样式列表
date: 2018-03-18 14:09:04
tags: CSS

---

一、列表中的默认信息

 	<ul> 和 <ol> 元素设置margin的顶部和底部: 16px(1em) 0;和 padding-left: 40px(2.5em); （在这里注意的是浏览器默认字体大小为16px）。
	<li> 默认是没有设置间距的。
	<dl> 元素设置 margin的顶部和底部: 16px(1em) ，无内边距设定。
	<dd> 元素设置为： margin-left  40px (2.5em)。
	在参考中提到的 <p>  元素设置 margin的顶部和底部: 16px(1em)，和其他的列表类型相同。


二、处理列表间距

当你创建样式列表时，需要调整样式，使其保持与周围元素相同的垂直间距和相互间的水平间距。用于文本样式和间距的CSS：

	/* General styles */
	html {
	  font-family: Helvetica, Arial, sans-serif;
	  font-size: 10px;
	}
	
	h2 {
	  font-size: 2rem;
	}
	
	ul,ol,dl,p {
	  font-size: 1.5rem;
	}
	
	li, p {
	  line-height: 1.5;
	}
	
	/* Description list styles */
	
	
	dd, dt {
	  line-height: 1.5;
	}
	
	dt {
	  font-weight: bold;
	}
	
	dd {
	  margin-bottom: 1.5rem;
	}
<span>
	
	第一条规则集设置一个网站字体，基准字体大小为10px。 页面上的所有内容都将继承该规则集。
	规则集2和3为标题、不同的列表类型和段落以及设置了相对字体大小（这些列表的子元素将会继承该规则集），这就意味着每个段落和列表都将拥有相同的字体大小和上下间距,有助于保持垂直间距一致。
	规则集4在段落和列表项目上设置相同的 line-height ，因此段落和每个单独的列表项目将在行之间具有相同的间距。 这也将有助于保持垂直间距一致。
	规则集5-7适用于描述列表 - 我们在描述列表的术语和其描述上设置与段落和列表项相同的行高，以及 margin-bottom 为1.5 rem（与段落（p）和列表项目（li））相同。 再次强调一遍，这里很好地实现了一致性！ 我们还使描述术语具有粗体字体，因此它们在视觉上脱颖而出。
	
三、列表特定样式

&lt;ol&gt;和&lt;ul&gt;元素上设置:
	
	list-style-type:设置用于列表的项目符号的类型，例如无序列表的方形或圆形项目符号，或有序列表的数字，字母或罗马数字。
	
	list-style-position:设置在每个项目开始之前，项目符号是出现在列表项内，还是出现在其外。
	
	list-style-image:允许您为项目符号使用自定义图片，而不是简单的方形或圆形。

1.符号样式

	/*有序列表上大写罗马数字*/
	ol {
	  list-style-type: upper-roman;
	}
2.项目符号位置

	ol {
	  list-style-type: upper-roman;
	  /*值设置为 inside，项目条目则位于行内；
	  默认值为 outside，项目符号位于列表项之外*/
	  list-style-position: inside;
	}
![inside和outside](position.jpg)

3.使用自定义的项目符号图片
	
	ul {
	  list-style-image: url(star.svg);
	}
这个属性在控制项目符号的位置，大小等方面是有限的。 您最好使用[background](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background) 系列属性，您将在 [Styling boxes](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Styling_boxes) 模块中了解更多信息。

eg:

	ul {
	  padding-left: 2rem;
	  list-style-type: none;
	}
	
	ul li {
	  padding-left: 2rem;
	  background-image: url(star.svg);
	  background-position: 0 0;
	  background-size: 1.6rem 1.6rem;
	  background-repeat: no-repeat;
	}
<span>

	将 <ul> 的 padding-left 从默认的 40px设置为 20px，然后在列表项上设置相同的数值。 这就是说，整个列表项仍然排列在有序列表项和描述列表中，但是列表项产生了一些用于背景图像的填充。 如果我们没有设置填充，背景图像将与列表项文本重叠，这看起来会很乱。
	将 list-style-type 设置为none，以便默认情况下不会显示项目符号。 我们将使用 background 属性来代替项目符号。
	为每个无序列表项插入项目符号，其相应的属性如下：
		background-image: 充当项目符号的图片文件的参考路径
		background-position: 这定义了所选元素背景中的图像将出现在哪里 - 在我们的示例中设置 0 0，这意味着项目符号将出现在每个列表项的最左上侧。
		background-size: 设置背景图片的大小。 理想条件下，我们想要项目符号与列表项的大小相同（比列表项稍大或稍小亦可）。 我们使用的尺寸为1.6rem（16px），它非常吻合我们为项目符号设置的 20px  的填充， 16px 加上 4px 的空格间距，可以使项目符号和列表项文本效果更好。
		background-repeat：默认条件下，背景图片不断复制直到填满整个背景空间，在我们的例子中，背景图片只需复制一次，所以我们设置值为 no-repeat。

4.list-style速记

上述提到的三种属性可以用一个单独的速记属性 list-style 来设置:

	ul {
	  list-style-type: square;
	  list-style-image: url(example.png);
	  list-style-position: inside;
	}
等价于

	ul {
	  list-style: square url(example.png) inside;
	}
属性值可以任意顺序排列，你可以设置一个，两个或者三个值（该属性的默认值为 disc, none, outside），如果指定了 type 和 image，如果由于某种原因导致图像无法加载，则 type 将用作回退

四、管理列表计数

在有序列表上进行不同的计数方式。例如： 从1以外的数字开始，或向后倒数，或者按步或多于1计数。

1.start属性：允许你从1 以外的数字开始计数

eg:从4开始依次+1：4，5，6，7

	<ol start="4">
	  <li>Toast pitta, leave to cool, then slice down the edge.</li>
	  <li>Fry the halloumi in a shallow, non-stick pan, until browned on both sides.</li>
	  <li>Wash and chop the salad.</li>
	  <li>Fill pitta with salad, humous, and fried halloumi.</li>
	</ol>
2.reversed属性：将启动列表倒计数

eg:从4开始依次-1:4，3，2，1

	<ol start="4" reversed>
	  <li>Toast pitta, leave to cool, then slice down the edge.</li>
	  <li>Fry the halloumi in a shallow, non-stick pan, until browned on both sides.</li>
	  <li>Wash and chop the salad.</li>
	  <li>Fill pitta with salad, humous, and fried halloumi.</li>
	</ol>
3.value属性:允许设置列表项指定数值

eg:显示指定内容：2，4，6，8

	<ol>
	  <li value="2">Toast pitta, leave to cool, then slice down the edge.</li>
	  <li value="4">Fry the halloumi in a shallow, non-stick pan, until browned on both sides.</li>
	  <li value="6">Wash and chop the salad.</li>
	  <li value="8">Fill pitta with salad, humous, and fried halloumi.</li>
	</ol>
	
参考资料：

1.[样式列表](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/为文本添加样式/Styling_lists)


