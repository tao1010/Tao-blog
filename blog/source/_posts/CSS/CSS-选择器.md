---
title: CSS-选择器
date: 2018-03-15 11:56:26
tags: CSS
---

一、不同种类的CSS选择器

选择器是CSS规则的一部分且位于CSS声明块前。

1.简单选择器

	通过元素类型、class 或 id 匹配一个或多个元素。
	
1.1元素选择器(类型选择器) - 这是选择所有指定类型的最简单方式。
	
一个选择器名和指定的HTML元素名的不区分大小写的匹配。

	//html	
	<p>What color do you like?</p>
	<div>I like blue.</div>
	<p>I prefer red!</p>

	//CSS
	/* All p elements are red */
	p {
	  color: red;
	}
	
	/* All div elements are blue */
	div {
	  color: blue;
	}
1.2类选择器

类选择器由一个点“.”以及类后面的类名组成。

类名是在HTML [class](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Global_attributes#attr-class)文档元素属性中没有空格的任何值。由你自己选择一个名字。

文档中的多个元素可以具有相同的类名，而单个元素可以有多个类名(以空格分开多个类名的形式书写)。
	
	//html
	<ul>
  		<li class="first done">Create an HTML document</li>
  		<li class="second done">Create a CSS style sheet</li>
  		<li class="third">Link them all together</li>
	</ul>
	
	//css
	/* The element with the class "first" is bolded */
	.first {
	  font-weight: bold;
	}
	
	/* All the elements with the class "done" are strike through */
	.done {
	  text-decoration: line-through;
	}
1.3ID选择器
	
<font color=red>选择单个元素的最有效的方式</font>:ID选择器由哈希/磅符号 (#)组成，后面是给定元素的ID名称。 任何元素都可以使用id属性设置唯一的ID名称。 由你自己选择的ID是什么。	
<font color=red>重要:</font>一个ID名称必须在文件中是唯一的。关于重复ID的行为是不可预测的，比如在一些浏览器只是第一个实例计算，其余的将被忽略.

	//html
	<p id="polite"> — "Good morning."</p>
	<p id="rude"> — "Go away!"</p>
	
	
	//css
	#polite {
	  font-family: cursive;
	}
	#rude {
	  font-family: monospace;
	  text-transform: uppercase;
	}
1.4通用选择器

通用选择（*）是最终的王牌。它允许选择在一个页面中的所有元素。由于给每个元素应用同样的规则几乎没有什么实际价值，更常见的做法是与其他选择器结合使用(参考下面 [组合](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Introduction_to_CSS/Simple_selectors#组合) .)

<font color=red>重要提示：</font>使用通用选择时小心。因为它适用于所有的元素，在大型网页利用它可以对性能有明显的影响：网页可以显示比预期要慢.

	<div>
  		<p>I think the containing box just needed
  		a <strong>border</strong> or <em>something</em>,
  		but this is getting <strong>out of hand</strong>!</p>
	</div>这是样式表：


	* {
		  padding: 5px;
		  border: 1px solid black;
		  background: rgba(255,0,0,0.25)
	}

2.属性选择器
	
一种特殊类型的选择器，它根据元素的 属性和属性值来匹配元素。

语法：由方括号([]) 组成，其中包含属性名称，后跟可选条件以匹配属性的值。
	
2.1存在和值(Presence and value)属性选择器

这些属性选择器尝试匹配精确的属性值：

	[attr]：该选择器选择包含 attr 属性的所有元素，不论 attr 的值为何。
	[attr=val]：该选择器仅选择 attr 属性被赋值为 val 的所有元素。
	[attr~=val]：该选择器仅选择 attr 属性的值（以空格间隔出多个值）中有包含 val 值的所有元素，比如位于被空格分隔的多个类（class）中的一个类。

eg:

	我的食谱配料: <i lang="fr-FR">Poulet basquaise</i>
	<ul>
	  <li data-quantity="1kg" data-vegetable>Tomatoes</li>
	  <li data-quantity="3" data-vegetable>Onions</li>
	  <li data-quantity="3" data-vegetable>Garlic</li>
	  <li data-quantity="700g" data-vegetable="not spicy like chili">Red pepper</li>
	  <li data-quantity="2kg" data-meat>Chicken</li>
	  <li data-quantity="optional 150g" data-meat>Bacon bits</li>
	  <li data-quantity="optional 10ml" data-vegetable="liquid">Olive oil</li>
	  <li data-quantity="25cl" data-vegetable="liquid">White wine</li>
	</ul>

	CSS
	
	/* 
    具有"data-vegetable"属性的所有元素,
    将被给予绿色的文本颜色
	*/
	[data-vegetable] {
	  color: green
	}
	
	/* 
	    具有"data-vegetable"属性且属性值刚好是"liquid"的所有元素，
	    将被给予金色背景颜色 
	*/
	[data-vegetable="liquid"] {
	  background-color: goldenrod;
	}
	
	/* 
	    具有"data-vegetable"属性且属性值包含"spicy"的所有元素，
	    即使某元素的该属性还包含其他属性值，
	    都会被给予红色的文本颜色 
	*/
	[data-vegetable~="spicy"] {
	  color: red;
	}
	
	本例中的 data-* 属性被称为 数据属性。它们提供了一种在HTML属性中存储自定义数据的方法，由此，这些数据可以轻松地被提取和使用。
![属性选择器1](Propertie-selector.png)

2.2子串值属性选择器

这种情况的属性选择器也被称为“伪正则选择器”，因为它们提供类似 regular expression 的灵活匹配方式（但请注意，这些选择器并不是真正的正则表达式）：

	[attr|=val] : 选择attr属性的值是 val 或值以 val- 开头的元素（-用来处理语言编码）。
	[attr^=val] : 选择attr属性的值以val开头（包括 val）的元素。
	[attr$=val] : 选择attr属性的值以val结尾（包括 val）的元素。
	[attr*=val] : 选择attr属性的值中包含字符串 val 的元素。

	html见上例子；
	
	CSS
	/* 语言选择的经典用法 */
	[lang|=fr] {
	  font-weight: bold;
	}
	
	/* 
	    具有"data-vegetable"属性含有值"not spicy"的所有元素,都变回绿色
	*/
	[data-vegetable*="not spicy"] {
	  color: green;
	}
	
	/* 
	   具有"data-quantity"属性其值以"kg"结尾的所有元素*/
	[data-quantity$="kg"] {
	  font-weight: bold;
	}
	
	/* 
	   具有属性"data-quantity"其值以"optional"开头的所有元素 
	*/
	[data-quantity^="optional"] {
	  opacity: 0.5;
	}	
3.伪类

	匹配处于确定状态的一个或多个元素，比如被鼠标指针悬停的元素，或当前被选中或未选中的复选框，或元素是DOM树中一父节点的第一个子节点。
	
	
4.伪元素
	
	配处于相关的确定位置的一个或多个元素，例如每个段落的第一个字，或者某个元素之前生成的内容。

[伪元素](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Introduction_to_CSS/Pseudo-classes_and_pseudo-elements)
	
	
	 
5.组合器
	
	这里不仅仅是选择器本身，还有以有效的方式组合两个或更多的选择器用于非常特定的选择的方法。例如，你可以只选择divs的直系子节点的段落，或者直接跟在headings后面的段落。
	
	
	
6.多用选取器

	这些也不是单独的选择器；这个思路是将以逗号分隔开的多个选择器放在一个CSS规则下面， 以将一组声明应用于由这些选择器选择的所有元素。






参考资料：

1.[选择器](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Introduction_to_CSS/Selectors)

2.[简单选择器](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Introduction_to_CSS/Simple_selectors)

3.[属性选择器](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Introduction_to_CSS/Attribute_selectors)

4.[伪类和伪元素](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Introduction_to_CSS/Pseudo-classes_and_pseudo-elements)

5.[组合器和多用选择器](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Introduction_to_CSS/Combinators_and_multiple_selectors)

6.[如何使用数据属性](https://developer.mozilla.org/zh-CN/docs/Web/Guide/HTML/Using_data_attributes)