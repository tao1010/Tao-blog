---
title: HTML基础知识-高级文本排版
date: 2018-03-09 11:59:02
tags: html
categories: Web
---

一、描述列表

1.[标记基本列表](https://tao1010.github.io/2018/03/08/Html基础知识-文本结构和内容/)

2.描述列表(description list)

	目的是标记一组项目及其相关描述，例如术语和定义，或者是问题和答案等。
	使用与其他列表类型不同的闭合标签— <dl> (描述列表：description list);
	每一项都用  <dt> (描述术语：description term)  元素闭合。 
	每个描述都用 <dd> (描述部分：description description) 元素闭合。

``` html
<dl>
	<dt>question1?</dt>
		<dd>In drama, where a character speaks to themselves, representing their inner thoughts or feelings and in the process relaying them to the audience (but not to other characters.)
		</dd>        
	<dt>question2?</dt>
       <dd>In drama, where a character speaks their thoughts out loud to share them with the audience and any other characters present.</dd>
   <dt>aside</dt>
       <dd>In drama, where a character shares a comment only with the audience for humorous or dramatic effect. This is usually a feeling, thought or piece of additional background information.</dd>
</dl>
```
tips:

	一个术语<dt>可以同时有多个描述<dd>
	
浏览器的默认样式会在描述列表的描述部分（description description）和描述术语（description terms）之间产生缩进。MDN非常严密地遵循这一惯例，同时也鼓励关于术语的其他更多的定义.

二、标记引文

1.块引用

如果一个块级内容(一个段落、多个段落、一个列表)从其他地方被引用，你应该把它用[&lt;blockquote&gt;](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/blockquote)元素包裹起来表示,并且在[&lt;cite&gt;](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/blockquote#attr-cite)属性里用URL来指向引用的资源。

``` html
<blockquote cite="https://developer.mozilla.org/en-US/docs/Web/HTML/Element/blockquote">
     <p>The <strong>HTML <code>&lt;blockquote&gt;</code> Element</strong> (or <em>HTML Block
            Quotation Element</em>) indicates that the enclosed text is an extended quotation.
      </p>
</blockquote>
```
浏览器在渲染块引用时默认会增加缩进，作为引用的一个指示符；MDN是这样做的，但是也增加了额外的样式：

2.行内引用

行内元素用同样的方式工作，除了使用&lt;q&gt;元素。

``` html
<p>The quote element — <code>&lt;q&gt;</code> — is <q cite="https://developer.mozilla.org/en-US/docs/Web/HTML/Element/q">intended
for short quotations that don't require paragraph breaks.</q></p>
```
	浏览器默认将其作为普通文本放入引号内表示引用;
	<q>元素旨在用于不需要分段的短引用
	
3.引文

引文默认的字体样式为斜体。浏览器、屏幕阅读器等等不会真的关心它，如果不使用JavaScript或CSS，浏览器不会显示cite的内容。如果你想要确保引用的资源在页面上是可用的，更好的方法是把<cite>元素放到引用元素旁边。这就意味着包含引用资源的名称——即引用的书的名字，或人的名字——但并不表示你不可以用同样的方式把要链接的文本放到<cite>元素中：

``` html
 <p>According to the <a href="https://developer.mozilla.org/en-US/docs/Web/HTML/Element/blockquote"> <cite>MDN blockquote page</cite></a>:</p>

    <blockquote cite="https://developer.mozilla.org/en-US/docs/Web/HTML/Element/blockquote">
      <p>The <strong>HTML <code>&lt;blockquote&gt;</code> Element</strong> (or <em>HTML Block Quotation Element</em>) indicates that the enclosed text is an extended quotation.</p>
    </blockquote>

    <p>The quote element — <code>&lt;q&gt;</code> — is <q cite="https://developer.mozilla.org/en-US/docs/Web/HTML/Element/q">intended for short quotations that don't require paragraph breaks.</q> -- <a href="https://developer.mozilla.org/en-US/docs/Web/HTML/Element/q"><cite>MDN q page</cite></a>.</p>
```
三、缩略语

&lt;abbr&gt;常常用来包裹一个缩略语或缩写，并提供缩写的解释(包含在title属性中,当光标移动到项目上时会出现提示)：

``` html
<p>We use <abbr title="Hypertext Markup Language">HTML</abbr> to structure our web documents.</p>

<p>I think <abbr title="Reverend">Rev.</abbr> Green did it in the kitchen with the chainsaw.</p>

```

四、标记联系方式

HTML有个用于标记联系方式的元素——&lt;address&gt;。它仅仅包含你的联系方式:

``` html
<address>
  <p>Chris Mills, Manchester, The Grim North, UK</p>
</address>
```

	<address>元素是为了标记编写HTML文档的人的联系方式，而不是任何其他的内容.
	
五、上标和下标

当你使用日期、化学方程式、和数学方程式时会偶尔使用上标和下标。 &lt;sup&gt; 和&lt;sub&gt;元素可以解决这样的问题。

``` html
<p>My birthday is on the 25<sup>th</sup> of May 2001.</p>
<p>Caffeine's chemical formula is C<sub>8</sub>H<sub>10</sub>N<sub>4</sub>O<sub>2</sub>.</p>
<p>If x<sup>2</sup> is 9, x must equal 3 or -3.</p>
```

六、展示计算机代码

有大量的HTML元素可以来标记计算机代码：

	<code>: 用于标记计算机通用代码。
	<pre>: 用于标记固定宽度的文本块，其中保留空格（通常是代码块）。
	<var>: 用于标记具体变量名。
	<kbd>: 用于标记输入电脑的键盘（或其他类型）输入。
	<samp>: 用于标记计算机程序的输出。


```html
<pre><code>var para = document.querySelector('p');

para.onclick = function() {
  alert('Owww, stop poking me!');
}</code></pre>

<p>You shouldn't use presentational elements like <code>&lt;font&gt;</code> and <code>&lt;center&gt;</code>.</p>

<p>In the above JavaScript example, <var>para</var> represents a paragraph element.</p>


<p>Select all the text with <kbd>Ctrl</kbd>/<kbd>Cmd</kbd> + <kbd>A</kbd>.</p>

<pre>$ <kbd>ping mozilla.org</kbd>
<samp>PING mozilla.org (63.245.215.20): 56 data bytes
64 bytes from 63.245.215.20: icmp_seq=0 ttl=40 time=158.233 ms</samp></pre>

```

七、标记事件和日期

HTML 还支持将时间和日期标记为可供机器识别的格式的 &lt;time&gt; 元素：

``` html
<!-- Standard simple date -->
<time datetime="2016-01-20">20 January 2016</time>
<!-- Just year and month -->
<time datetime="2016-01">January 2016</time>
<!-- Just month and day -->
<time datetime="01-20">20 January</time>
<!-- Just time, hours and minutes -->
<time datetime="19:30">19:30</time>
<!-- You can do seconds and milliseconds too! -->
<time datetime="19:30:01.856">19:30:01.856</time>
<!-- Date and time -->
<time datetime="2016-01-20T19:30">7.30pm, 20 January 2016</time>
<!-- Date and time with timezone offset-->
<time datetime="2016-01-20T19:30+01:00">7.30pm, 20 January 2016 is 8.30pm in France</time>
<!-- Calling out a specific week number-->
<time datetime="2016-W04">The fourth week of 2016</time>

```


参考资料：

1.[HTML高级文本排版](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Introduction_to_HTML/Advanced_text_formatting)

2.[Address](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/address)