---
title: CSS-文本样式-样式化链接
date: 2018-03-19 09:37:57
tags: CSS
---

一、链接

1.链接状态

链接存在时处于不同的状态，每一个状态都可以用对应的[伪类](https://developer.mozilla.org/zh-CN/Learn/CSS/Introduction_to_CSS/Selectors#Pseudo-classes)来应用样式:

	Link (没有访问过的): 这是链接的默认状态，当它没有处在其他状态的时候，它可以使用:link 伪类来应用样式。
	Visited: 这个链接已经被访问过了(存在于浏览器的历史纪录), 它可以使用 :visited 伪类来应用样式。
	Hover: 当用户的鼠标光标刚好停留在这个链接，它可以使用 :hover 伪类来应用样式。
	Focus: 一个链接当它被选中的时候 (比如通过键盘的 Tab  移动到这个链接的时候，或者使用编程的方法来选中这个链接 HTMLElement.focus()) 它可以使用 :focus 伪类来应用样式。
	Active: 一个链接当它被激活的时候 (比如被点击的时候)，它可以使用 :active 伪类来应用样式。
2.默认样式

	<p><a href="https://mozilla.org">A link to the Mozilla homepage</a></p>
<span>

	p {
	  font-size: 2rem;
	  text-align: center;
	}
<span>

	• 链接具有下划线；
	• 未访问过的 (Unvisited) 的链接是蓝色的。
	• 访问过的 (Visited) 的链接是紫色的.
	• 悬停 (Hover) 在一个链接的时候鼠标的光标会变成一个小手的图标。
	• 选中 (Focus) 链接的时候，链接周围会有一个轮廓，你应该可以按 tab 来选中这个页面的链接 (在 Mac 上, 你可能需要使用Full Keyboard Access: All controls 选项，然后再按下 Ctrl + F7 ，这样就可以起作用)
	• 激活 (Active) 链接的时候会变成红色 (当你点击链接时，请尝试按住鼠标按钮。)
保持链接的传统，不应该与用户预期相差太大：

	• 为链接使用下划线，但是不要在其他内容上也用下划线，以作区分。如果你不想要带有下划线的链接，那你至少要用其他方法来高亮突出链接。
	• 当用户悬停或选择 (hover 或者 focused) 的时候，使链接有相应的变化，并且在链接被激活(active) 的时候，变化会有一些不同。可以使用以下CSS属性关闭/更改默认样式：
		• color 文字的颜色
		• cursor 鼠标光标的样式，你不应该把这个关掉，除非你有非常好的理由。
		• outline 文字的轮廓 (轮廓有点像边框，唯一的区别是边框占用了盒模型的空间，而轮廓没有； 它只是设置在背景图片的顶部)。outline 有一个有用的辅助功能，所以在把它关掉之前考虑清楚；你应该至少将 选中 (focus) 状态上链接悬停 (hover) 状态的样式加倍。
3.将样式应用到一些链接

顺序很重要，不能任意变更：LoVe Fears HAte.

	L（link）oV(visited)e F(focus)ears H(hover)A(active)te.
	/*链接设置顺序*/
	p {
	  line-height: 1.4;
	}
	
	/*a选择器取消了默认文本下划线和链接被选中(focus)时的轮廓(outline)(不同浏览器默认行为可能不同，不过这里取消了),
	并为每个链接添加了少量的内边距(padding)*/
	a {
	  outline: none;
	  text-decoration: none;
	  padding: 2px 1px 0;
	}
	/*	使用a:link选择器来设置未访问(unvisited)链接的一些颜色变化*/
	a:link {
	  color: #265301;
	}
	/*使用a:visited选择器来设置访问过(visited)的链接上的一些颜色变化*/
	a:visited {
	  color: #437A16;
	}
	/*选中:设置鼠标选中链接的背景颜色，下划线使链接更加突出*/
	a:focus {
	  border-bottom: 1px solid;
	  background: #BAE498;
	}
	/*悬停：设置鼠标悬停时链接的背景颜色和下划线时链接更加突出*/
	a:hover {
	  border-bottom: 1px solid;     
	  background: #CDFEAA;
	}
	/*给链接一个不同的配色方案,当链接被激活(activated)时,让链接被激活的时候更加明显*/
	a:active {
	  background: #265301;
	  color: #CDFEAA;
	}

<font color=red>注意:</font>在上述的a:focus和a:hover的设置中：
	
	下划线是使用 border-bottom 创造的, 而不是 text-decoration，有一些人喜欢这个，因为前者比后者有更好的样式选项， 并且绘制的位置会稍微低一点，所以不会穿过字母 (比如 字母 g 和 y 底部).
	这个 border-bottom 值被设置为1px solid, 没有明确的颜色。这样做可以使边框采用和元素文本一样的颜色，这在这样的情况下是很有用的，因为链接的每种状态下，文本是不同的颜色。

二、链接中的图标

外部链接：链接指向的不是本站，而是外部站点。

	<p>For more information on the weather, visit our <a href="weather.html">weather page</a>,
	look at <a href="https://en.wikipedia.org/wiki/Weather">weather on Wikipedia</a>, or check
	out <a href="http://www.extremescience.com/weather.htm">weather on Extreme Science</a>.</p>
<span>

	body {
	  width: 300px;
	  margin: 0 auto;
	  font-family: sans-serif;
	}
	
	p {
	  line-height: 1.4;
	}
	
	a {
	  outline: none;
	  text-decoration: none;
	  padding: 2px 1px 0;
	}
	
	a:link {
	  color: blue;
	}
	
	a:visited {
	  color: purple;
	}
	
	a:focus, a:hover {
	  border-bottom: 1px solid;
	}
	
	a:active {
	  color: red;
	}
	/**
	1.使用了 background 简写代替了使用多个属性。
		设置想要插入的图片的路径，
		指定 no-repeat ，只得到了一个插入的副本，
		然后指定位置是 100% 使其出现在内容的右边，
		然后距离上方是 0 px
	2.使用 background-size 来指定要显示的背景图像的大小，这样对有一个大的图标，然后重新调整它的大小是很有帮助的，也是响应式网站设计的需要。(适用于IE 9及更高版本,如果需要支持那些老的浏览器，你只需要调整图像大小，然后插入它)
	
	3.设置 padding-right ，为背景图片留出空间，所以我们不会和文本重叠;
	
	4.如何只选中外部链接的："http" 文本应该只出现在外部链接上，所以我们可以使用一个属性选择器 attribute selector: a[href*="http"] 选中 <a> 元素，但是只会选中那些拥有 href 属性，且属性的值包含 "http" 的 <a>的元素。
	*/
	a[href*="http"] {
	  background: url('https://mdn.mozillademos.org/files/12982/external-link-52.png') no-repeat 100% 0;
	  background-size: 16px 16px;
	  padding-right: 19px;
	}

三、样式化链接为按钮

应用场景：

	1.悬停 (hover) 的状态可以为不同的元素应用样式，不只是链接，你也许会想添加悬停状态的样式到段落、列表项、或者是其他东西；
	2.在某些情况下，链接通常会应用样式，使它看上去的效果和按钮差不多，一个网站导航菜单通常是标记为一个列表，列表中包含链接，这可以很容易地被设计为看起来像一组控制按钮或是选项卡，主要是用于让用户可以访问站点的其他部分。
eg:
	
	<ul>
	  <li><a href="#">Home</a></li><li><a href="#">Pizza</a></li><li><a href="#">Music</a></li><li><a href="#">Wombats</a></li><li><a href="#">Finland</a></li>
	</ul>

<span>

	body,html {
	  margin: 0;
	  font-family: sans-serif;
	}
	/*删除了 <ul> 元素的默认的 padding，然后设置了它的宽度是外部容器  <body> (在这次条件下) 的 100% */
	ul {
	  padding: 0;
	  width: 100%;
	}
	/*<li> 元素通常默认是块元素，意味着它们各自会占用一行.
	在这个例子中，创建了一组水平列表的链接，所以在第三条规则中，设置了 display 属性为 inline，这会导致列表中的每项内容都会一起出现在同一行，它们现在表现得就像内联元素*/
	li {
	  display: inline;
	}
	/**<a>元素的样式
		1.首先关掉了 text-decoration 和 outline;
		2.设置 display 为 inline-block ，<a> 元素默认为内联元素，而且我们不希望它们像值为 block 时一样，线条超出自己的内容，我们确实想要控制它们的大小inline-block 允许我们这样做;
		3.接着是尺寸的设置! 要填满整个 <ul> 的宽度，为按钮之间留一些间距 (margin)  (但不是右边边缘的间距)，我们有 5 个按钮需要容纳，所以它们的大小应该一样。为了做到这一点，我们设置 width 为 19.5%，然后 margin-right 为 0.625%. 你会注意到所有宽度加起来是 100.625%, 这样会让最后一个按钮溢出 <ul> ，然后显示到下一行中。但是，我们使用了下一条规则让它恢复到了 100%，这条规则选中了列表中的最后一个 <a>元素，然后删除了它的间距 (margin);
		4.主要是为链接各个状态添加了颜色。我们居中了每个链接中的文本，设置 line-height 为 3， 让按钮有一些高度 (这也具有垂直居中文本的优点)，并设置文本的颜色为黑色;
	*/
	a {
	  outline: none;
	  text-decoration: none;
	  display: inline-block;
	  width: 19.5%;
	  margin-right: 0.625%;
	  text-align: center;
	  line-height: 3;
	  color: black;
	}
	
	li:last-child a {
	  margin-right: 0;
	}
	
	a:link, a:visited, a:focus {
	  background: yellow;
	}
	
	a:hover {     
	  background: orange;
	}
	
	a:active {
	  background: red;
	  color: white;
	}

![linklikebutton](linklikebutton.png)


<font color=red>注意:</font>HTML 中的列表的每项内容都在同一行上，这是因为 inline-block 元素在页面上创建的空格换行符，就像几个字之间的空格，这样的空隙也许会破坏我们的水平导航菜单布局。所以我们删除了空格。(详见参考资料3）








参考资料：

1.[样式化链接](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/为文本添加样式/Styling_links)

2.[backgrounds](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Styling_boxes)

3.[Fighting the space between inline block elements ](https://css-tricks.com/fighting-the-space-between-inline-block-elements/)

4.[type of CSS boxes](https://developer.mozilla.org/zh-CN/Learn/CSS/Introduction_to_CSS/Box_model#Types_of_CSS_boxes)