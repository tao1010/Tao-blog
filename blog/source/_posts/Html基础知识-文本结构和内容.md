---
title: Html基础知识-文本结构和内容
date: 2018-03-08 13:33:46
tags: html
---

一、标题和段落

1.段落 &lt;p&gt;段落内容&lt;/p&gt;

``` html
<p>I am a paragraph, oh yes I am.</p>
```

2.标题 

每个标题（Heading）是通过“标题标签”进行定义的：

``` html
<h1>I am the title of the story.</h1>
```

	这里有六个标题元素标签— <h1>, <h2>, <h3>, <h4>, <h5>,<h6>. 
	每个元素代表文档中不同级别的内容; <h1>表示主标题（the main heading），<h2>表示二级子标题（subheadings），<h3>表示三级子标题（sub-subheadings），等等。
	
3.编辑结构层次

eg:
	
	在一个故事中，<h1>表示故事的名字，<h2>表示每个章节的标题， <h3>表示每个章节下的子标题，以此类推。

所涉及的元素具体代表什么，完全取决于作者编辑的内容，只要层次结构是合理的。在创建此类结构时，您只需要记住一些最佳实践：

	1.优选地，您应该只对每个页面使用一次<h1> — 这是顶级标题，所有其他标题位于层次结构中的下方。
	2.请确保在层次结构中以正确的顺序使用标题。不要使用<h3>来表示副标题，后面跟<h2>来表示副副标题 - 这是没有意义的，会导致奇怪的结果。
	3.在可用的六个标题级别中，您应该旨在每页使用不超过三个，除非您认为有必要使用更多。具有许多级别的文档（即，较深的标题层次结构）变得难以操作并且难以导航。在这种情况下，如果可能，建议将内容分散在多个页面上。
	
二、列表(Lists)
	
1.无序(Unordered) - &lt;ul&gt;unordered lists&lt;/ul&gt;

无序的列表被用来标记每个项目。在这里，项目的顺序并不重要.

``` html
<ul>
<li>牛奶</li>
<li>鸡蛋</li>
<li>面包</li>
</ul>
```

2.有序(Ordered) - &lt;ol&gt;ordered lists&lt;/ol&gt;
	
有序的列表是根据项目的顺序列出来的.

``` html
<ol>
  <li>行驶到这条路的尽头</li>
  <li>向右转</li>
  <li>直行穿过第一个双环形交叉路</li>
  <li>在第三个环形交叉路左转</li>
  <li>学校就在你的右边，300米处</li>
</ol>
```

3.嵌套列表Nesting lists

将一个列表嵌入到另一个列表是完全可以的，

``` html
<ol>
  <li>这是第一步</li>
  <li>这是第二步</li>
  <li>这是第三步</li>
  <li>这是第四步
    <ul>
      <li> AAAAAA</li>
      <li> BBBBBB</li>
    </ul>
  </li>
</ol>
```

4.重点强调

强调

在HTML中我们用[&lt;em&gt;（emphasis）](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/em)元素来标记这样的情况。这样做既可以让文档读起来更有趣，也可以被屏幕阅读器识别出来，并以不同的语调发出。浏览器默认风格为斜体，但你不应该纯粹使用这个标签来获得斜体风格，为了获得斜体风格，你应该使用[&lt;span&gt;](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/span)元素和一些CSS，或者是[&lt;i&gt;](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/i)元素（见下文）。

``` html
<p>I am <em>glad</em> you weren't <em>late</em>.</p>
```

&lt;em&gt;和&lt;i&gt;的区别：
	
	在 默认情况下，视觉效果是一样的 - 这两个标签都把内容呈现为斜体. 
	语义是不同的： <em> 标签表示着重强调其内容，而 <i> 标签表示从正常的散文中区分出的文本, 如电影或书籍的名字，一个外来词, 或者当文本指的是一个字的定义，而不是其自身代表的语义。
	
&lt;span&gt;和&lt;div&gt;的区别：

	<span> 元素是行内元素，是短语内容的通用行内容器，并没有任何特殊语义。可以使用它来编组元素以达到某种样式意图（通过使用类或者Id属性），或者这些元素有着共同的属性，比如lang。应该在没有其他合适的语义元素时才使用它。
	
	<div> 是块元素
		
	不允许省略标签（开始，结束都不能省略）

非常重要
	
在文字方面则是用粗体字来达到强调的效果。&lt;strong&gt;(strong importance) 元素来标记这样的请况。浏览器默认风格为粗体，但你不应该纯粹使用这个标签来获得粗体风格，为了获得粗体风格，你应该使用&lt;span&gt;元素和一些CSS，或者是&lt;b&gt; 元素 (见下文)。

``` html
<p>This liquid is <strong>highly toxic</strong>.</p>
<p>I am counting on you. <strong>Do not</strong> be late!</p>
```

斜体、粗体、下划线...

	这里是最好的经验法则：使用<b>,<i>,<u> 来传达传统意义上的粗体，斜体或下划线是合适的
	<i> 被用来传达传统上用斜体表达的意义：外国文字，分类名称，技术术语，一种思想……
	<b> 被用来传达传统上用粗体表达的意义：关键字，产品名称，引导句……
	<u> 被用来传达传统上用下划线表达的意义：专有名词，拼写错误……


参考资料：

1.[文本结构和内容](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Introduction_to_HTML/HTML_text_fundamentals)