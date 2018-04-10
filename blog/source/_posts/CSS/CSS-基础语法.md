---
title: CSS-基础语法
date: 2018-03-15 10:52:51
tags: CSS
categories: Web
---

CSS 是一种声明式语言，这令其语法相当简单直观且易于理解。此外，CSS还有着相当不错的错误修复系统，这一特性允许你犯一些错误但并不会造成什么异常影响 —— 比如，有可能你的某些声明浏览器并不理解，这时浏览器会直接忽略掉这些内容并继续执行渲染。当然这一特性也可能导致一些问题，比如一些异常情况出现的时候，可能在定位问题原因上造成一些障碍。随着阅读的深入，这些优势和缺点都将逐渐清晰。

一、概念介绍

1.CSS组成：属性(Propertie) + 属性值(Value)
	
	属性（Propertie）：一些人类可理解的标识符，这些标识符指出你想修改哪一些样式，例如：字体，宽度，背景颜色等。
	
	属性（Propertie）：一些人类可理解的标识符，这些标识符指出你想修改哪一些样式，例如：字体，宽度，背景颜色等。
	
	CSS 的属性和属性值都是区分大小写的。属性和属性值之间，用英文半角冒号 (:) 隔离。
	
	属性和属性值不能任意组合：每个属性都有一个已经定义好的可用属性值范围。 
	
<font color=red>重要1：</font>	如果使用了未知属性，或者给属性赋予了无效值，该声明会被视为无效，浏览器的 CSS 引擎会完全忽略它。
	
<font color=red>重要2：</font>在 CSS（和其他网络标准）中，使用美式拼写作为单词的标准写法。例如，颜色（见于上述代码所见）应始终拼写为 color。写成 colour 会无法正常工作。
	
	
2.声明

![CSS声明](CSS-statement.png)

与值配对的属性被称为CSS声明。CSS声明会被放置在一个CSS声明块中。最后，CSS声明块与选择器相结合形成一个CSS规则集（或CSS规则）。

给 CSS 属性设置特定的值是 CSS 语言的核心功能。CSS 引擎会通过计算，将对应的 CSS 声明应用到页面的每一个元素上，从而使得元素们以适当的方式布局，并展示出适当的样式。

3.声明块

![CSS声明块](CSS-statement-block.png)

声明被按块分组,每一组都用一对{}包裹，用{表示开始,用}表示结束;

声明块里的每一组声明必须用半角分号(;)分割，否则代码不生效（至少不会按预期结果生效）。声明块里的最后一个声明结束的地方，不需要加分号，但是最后加分号是个好习惯，因为可以防止在后续增加声明时忘记加分号。

	块有时候是可以嵌套的。这种情况下，每一对括号必须逻辑上嵌套，跟嵌套 HTML 元素的标签嵌套方式相同。最常见的例子是 @-rules，这是一种用 @ 标识开头的块，例如 @media, @font-face等;
	
	声明块的内容允许为空 —— 这完全有效。
4.CSS选择器和规则

通过在每个声明块前加上选择器（selector）来完成这一动作，选择器是一种模式，它能在页面上匹配一些元素。这将使相关的声明仅被应用到被选择的元素上。选择器加上声明块被称为规则集（ruleset），通常简称规则（rule）。

![CSS-selector](CSS-selector.png)

[选择器](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Introduction_to_CSS/Selectors)可以很复杂 —— 你可以制作一个匹配多种元素的规则，这是通过把多个选择器囊括成用逗号分隔的选择器（一组,）来达成；选择器还可以被“链”在一起，例如我要选择所有 class 是 "blah" 的元素, 但是当且仅当它在 <article> 元素里、并且仅当鼠标指针悬停在这个元素上的时候。

一个元素可以被多个选择器所匹配，因此，一个给定的属性可能被多个规则设置多次。 CSS 定义了哪个规则比其它规则更具优先级，则更具优先级的规则必定被应用：这被称为层叠算法（cascade algorithm），关于层叠算法的更多内容和运作原理见[层叠和继承](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Introduction_to_CSS/Cascade_and_inheritance)。

<font color=red>重要：</font>如果链或组中的某个选择器无效，比如使用了未知的伪元素或伪类，整个组的选择器仍然是有效的，除了这个无效的将被忽略的选择器。

5.CSS语句

CSS语句：
	
	1.CSS规则(CSS语句中的一种) - 是样式表的主要组成块;在CSS中最常见的块;
	
	2.@-规则(At-rules) - 在CSS中被用来传递元数据、条件信息或其它描述性信息。
	它由（@）符号开始，紧跟着一个表明它是哪种规则的描述符，之后是这种规则的语法块，并最终由一个半角分号（;）结束。每种由描述符定义的@-规则，都有其特有的内部语法和语义。
	eg1:@charset和@import（元数据）
	eg2:@media 或@document(条件信息，又被称为嵌套语句)
	eg3:@font-face（描述性信息）
	该@-规则向当前 CSS 导入其它 CSS 文件。
	eg：@import 'custom.css'

	3.嵌套语句 - 是@-规则中的一种，它的语法是 CSS 规则的嵌套块，只有在特定条件匹配时才会应用到文档上。特定条件如下：
	@media 只有在运行浏览器的设备匹配其表达条件时才会应用该@-规则的内容；
	@supports 只有浏览器确实支持被测功能时才会应用该@-规则的内容；
	@document 只有当前页面匹配一些条件时才会应用该@-规则的内容。
	
	eg：下列嵌套语句只有在页面宽度超过801像素时才会应用.
	@media(min-width: 801px){
		body{
			
			margin: 0auto;
			width: 800px;
		}
	}
<font color=red>重要：</font>任何不是规则集或@-规则或嵌套语句的 CSS 语句都是无效的，并会因此被忽略。

二、其他

1.空格

	空格实际上意味着一些空格，一些制表符以及一些新行。你可以通过添加空格使你的样式表更具可读性;

	浏览器会倾向于忽略你 CSS 中的许多空格；许多空格在那的意义仅仅是增加了可读性;

	始终确保至少用一个空格分隔不同的值，且保持属性名/值为一个连续的字符串;


2.注释

	CSS中的注释以 /* 开始并以 */ 结束;
	
3.简写

	一些属性比如 font，background，padding，border，和 margin 被称为简写属性—— 这是由于它们允许你在一行设置多个属性，从而节省时间并使代码更整洁;

	eg1:
	/* 在padding和margin这样的简写属性中，值赋值的顺序是top、right、bottom、left。 
	它们还有其他简写方式，例如给padding两个值，则第一个值表示top/bottom，第二个值表示left/right */
	padding: 10px 15px 15px 5px;
	等价于：
	padding-top: 10px;
	padding-right: 15px;
	padding-bottom: 15px;
	padding-left: 5px;
	
	
	eg2:
	background: red url(bg-graphic.png) 10px 10px repeat-x fixed;
	等价于：
	background-color: red;
	background-image: url(bg-graphic.png);
	background-position: 10px 10px;
	background-repeat: repeat-x;
	background-scroll: fixed;
	
	

参考资料：

1.[CSS语法](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Introduction_to_CSS/Syntax)

2.[@-规则](https://developer.mozilla.org/zh-CN/docs/Web/CSS/At-rule)
