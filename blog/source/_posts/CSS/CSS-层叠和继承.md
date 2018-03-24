---
title: CSS-层叠和继承
date: 2018-03-16 14:21:27
tags: CSS
category: Web

---

当多个选择器作用在一个元素上时，哪个规则最终会应用到元素上？这是通过层叠机制来控制的，这也和样式继承(元素从其父元素那里获得的属性值)有关。

一、层叠

CSS 是 Cascading Style Sheets 的缩写，这暗示层叠（cascade）的概念是很重要的。在最基本的层面上，它表明CSS规则的顺序很重要，但它比那更复杂。什么选择器在层叠中胜出取决于三个因素（这些都是按重量级顺序排列的——前面的的一种会否决后一种）：

	1.重要性（Importance）
	2.专用性（Specificity）
	3.源代码次序（Source order）


1.重要性

在CSS中，属性值后面加上 !important 可以让这条规则优先于其他规则：<br>
eg:
	
	<p class="better">This is a paragraph.</p>
	<p class="better" id="winning">One selector to rule them all!</p>

	CSS
	#winning {
	  background-color: red;
	  border: 1px solid black;
	}
	
	.better {
	  background-color: gray;
	  border: none !important;//语法格式
	}
	
	p {
	  background-color: blue;
	  color: white;
	  padding: 5px;
	}

![important](important.png)

	• 第三条规则 color 和 padding 被运用了, 但 background-color没有:实际上，这三种情况都应该应用，因为在源顺序后面的规则通常会覆盖较早的规则。
	• 然而, 在前面的规则被运用了,因为IDs/class 选择器优先于element选择器;
	• 这两个元素都有 class并带有 better属性, 但是第二个元素有 id 值为winning 。 因为比起class而言id专用性更高(在一个页面上id是唯一的, 但很多元素可以拥有相同的class — ID 选择器在它们的目标中是非常优先的)，红色背景色和1pixel的黑色边框都应应用于第二元素，第一个元素获得灰色背景色，没有边框，如类所指定。
	• 第二个元素获得红色背景色，但没有边框。因为 !important 在第二条规则中的声明——在 border: none之后写入它意味着尽管id具有更高的优先性，该声明也将优先于前面规则中的边界值声明。
<font color=red>注意:</font>重载这个 !important 声明的唯一方法是在后面的源码或者是一个拥有更高特殊性的源码中包含相同的 !important 特性的声明。

应用说明：
	
	• !important存在是很有用的，但是！建议千万不要使用它，除非绝对必须使用它;
	• 不得不使用的情况：在CMS中工作时，不能编辑核心的CSS模块，并且确实想要重写一种不能以其他方式覆盖的样式;
	• 由于 !important 改变了层叠正常工作的方式，因此调试CSS问题，尤其是在大型样式表中，会变得非常困难。

用户可以设置自定义样式表覆盖开发商的样式,比如:视力障碍者，调整字体大小。

相互冲突的声明将按以下顺序适用，后一种将覆盖先前的声明：

	a)在用户代理样式表的声明 (例如：浏览器在没有其他声明的默认样式).
	b)用户样式表中的普通声明（由用户设置的自定义样式）。
	c)作者样式表中的普通声明（这是我们设置的样式，Web开发人员）。
	d)作者样式表中的重要声明
	e)用户样式表中的重要声明
	
2.专用性

专用性基本上是衡量选择器的具体程度的一种方法——它能匹配多少元素。
优先级:

	!important语法 > ID选择器 > 类选择器 > 元素选择器

一个选择器具有的专用性的量是用四种不同的值（或组件）来衡量的，它们可以被认为是千位，百位，十位和个位——在四个列中的四个简单数字：

	• 千位：如果声明是在style 属性中该列加1分（这样的声明没有选择器，所以它们的专用性总是1000。）否则为0。
	• 百位：在整个选择器中每包含一个ID选择器就在该列中加1分。
	• 十位：在整个选择器中每包含一个类选择器、属性选择器、或者伪类就在该列中加1分。
	• 个位：在整个选择器中每包含一个元素选择器或伪元素就在该列中加1分。

tips:通用选择器(*),复合选择器(+,>,~,''),否定伪类(:not)在专用性中无影响。

![example](example.png)

eg:
	
	HTML
	<div id="outer" class="container">
	  <div id="inner" class="container">
	    <ul>
	      <li class="nav"><a href="#">One</a></li>
	      <li class="nav"><a href="#">Two</a></li>
	    </ul>
	  </div>
	</div>

	CSS
	/* specificity: 0101 */
	/*id(outer - 0100) + 元素(a- 0001) = 0101  */
	#outer a {	  
	  background-color: red;
	}

	/* specificity: 0201 */
	/*id(outer|inner - 0100 + 0100) + 元素(a- 0001) = 0201  */
	#outer #inner a {
	  background-color: blue;
	}
	
	/* specificity: 0104 */
	/*id(outer - 0100) + 元素(a|div|ul|li- 0001 * 4) = 0104  */
	#outer div ul li a {
	  color: yellow;
	}
	
	/* specificity: 0113 */
	/*id(outer - 0100) + 元素(a|ul|div- 0001 * 3) + 类选择器(class - 0010) = 0113  */
	#outer div ul .nav a {
	  color: white;
	}
	
	/* specificity: 0024 */
	/*伪类(:hover|:nth-child(2) - 0010 * 2) + 元素(div|div|li|a- 0001* 4) = 0024  */
	div div li:nth-child(2) a:hover {
	  border: 10px solid black;
	}
	
	/* specificity: 0023 */
	/*伪类(:hover|:nth-child(2) - 0010 * 2) + 元素(div|li|a- 0001* 3) = 0023  */
	div li:nth-child(2) a:hover {
	  border: 10px dashed black;
	}
	
	/* specificity: 0033 */
	/* 类选择器(.nav - 0010) + 伪类(:hover|:nth-child(2) - 0010 * 2) + 元素(div|div|a- 0001 * 3) = 0033  */
	div div .nav:nth-child(2) a:hover {
	  border: 10px double black;
	}
	
	a {
	  display: inline-block;
	  line-height: 40px;
	  font-size: 20px;
	  text-decoration: none;
	  text-align: center;
	  width: 200px;
	  margin-bottom: 10px;
	}
	
	ul {
	  padding: 0;
	}
	
	li {
	  list-style-type: none;
	}


![specificity](specificity.png)

	• 前两个选择器正在竞争链接的背景颜色的样式——第二个赢得并使背景色为蓝色:专用性值为101比201;
	• 第三个和第四个选择器在链接文本颜色的样式上进行竞争——第二个选择器获胜，使文本变白:专用性值为104和113;
	• 第五、六、七个选择器在徘徊在链接附近时的样式进行竞争。专用性值 - 五：六：七 = 24：23：33 - 第七个选择器胜;
3.源代码次序

如果多个相互竞争的选择器具有相同的重要性和专用性，那么第三个因素将帮助决定哪一个规则获胜

eg1:后面的规则将战胜先前的规则(第二个选择器优先)

	p {
	  color: blue;
	}
	
	/* This rule will win over the first one */
	p {
	  color: red;
	}

eg2:专用性高于源代码顺序(第一个选择器优先)
	
	/* This rule will win */
	.footnote {
	  color: blue;
	}
	
	p {
	  color: red;
	}

4.在考虑所有这些层叠理论和什么样式优先于其他样式被应用时，记住：所有这些都发生在属性级别上——属性覆盖其他属性，但不会让整个规则凌驾于其他规则之上。

当多个CSS规则匹配相同的元素时，它们都被应用到该元素中。只有在这之后，任何相互冲突的属性才会被评估，以确定哪种风格会战胜其他类型。
	
	<p>I'm <strong>important</strong></p>
	
	/* specificity: 0002 */
	p strong {
	  background-color: khaki;
	  color: green;
	}
	
	/* specificity: 0001 */
	strong {
	  text-decoration: underline;
	  color: red;
	}
![hybrid](hybrid.png)

二、继承

CSS继承以获取所有信息并了解什么样式应用于元素。其思想是，应用于某个元素的一些属性值将由该元素的子元素继承，而有些则不会。例如:

	1.对 font-family 和 color 进行继承是有意义的;
	因为这使得您可以很容易地设置一个站点范围的基本字体，方法是应用一个字体到 <html> 元素；然后，您可以在需要的地方覆盖单个元素的字体。如果要在每个元素上分别设置基本字体，那就太麻烦了。

	2.让 margin，padding，border 和 background-image 不被继承是有意义的;
	因为如果您将这些属性设置在一个容器元素上，并将它们继承到每个子元素，然后不得不将它们全部放在每个单独的元素上，那么将会出现的样式/布局混乱。
1.控制继承

CSS为处理继承提供了三种特殊的通用属性值：

	inherit： 该值将应用到选定元素的属性值设置为与其父元素一样。(最有趣 - 它允许我们显式地让一个元素从其父类继承一个属性值.)
	initial ：该值将应用到选定元素的属性值设置为与浏览器默认样式表中该元素设置的值一样。如果浏览器默认样式表中没有设置值，并且该属性是自然继承的，那么该属性值就被设置为 inherit。
	unset ：该值将属性重置为其自然值，即如果属性是自然继承的，那么它就表现得像 inherit，否则就是表现得像 initial。

<font color=red>tips:</font> initial 和 unset 不被IE支持。

eg:

	<ul>
	  <li>Default <a href="#">link</a> color</li>
	  <li class="inherit">Inherit the <a href="#">link</a> color</li>
	  <li class="initial">Reset the <a href="#">link</a> color</li>
	  <li class="unset">Unset the <a href="#">link</a> color</li>
	</ul>
	
	CSS
	body {
	  color: green;
	}
	
	.inherit a {
	  color: inherit;
	}
	
	.initial a {
	  color: initial
	}
	
	.unset a {
	  color: unset;
	}
![inheritance](inheritance.png)

	a)首先设置<body> 的color为绿色。
	b)由于color属性是自然继承的，所有的body子元素都会有相同的绿色。需要注意的是默认情况下浏览器设置链接的颜色为蓝色，而不是自然继承color属性，因此在我们列表中的第一个链接是蓝色的。
	c)第二个规则设置一个类 inherit 的元素内的链接，并从父类继承它的颜色。在这种情况下, 意思是说链接继承了父元素<li>的颜色，默认情况下<li>的颜色来自于它的父元素 <ul> , 最后<ul> 继承自 <body>元素，而<body>的color 根据第一条规则设置成了绿色。
	d)第三个规则选择了在元素上使用类 initial 的任意链接然后设置他们的颜色为 initial 。通常， initial 的值被浏览器设置成了黑色，因此该链接被设置成了黑色。
	e)最后一个规则选择了在元素上使用类 unset 的所有链接然后设置它们的颜色为 unset  ——即我们不设置值。因为color属性是一个自然继承的属性，它实际上就像把值设置成 inherit 一样。结果是，该链接被设置成了与body一样的颜色——绿色。






参考资料:

1.[层叠和继承](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Introduction_to_CSS/Cascade_and_inheritance)

2.[CSS参考](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Reference)

3.[inherit继承属性](https://developer.mozilla.org/zh-CN/docs/Web/CSS/inherit)

4.[initial继承属性](https://developer.mozilla.org/zh-CN/docs/Web/CSS/initial)

5.[unset继承属性](https://developer.mozilla.org/zh-CN/docs/Web/CSS/unset)

