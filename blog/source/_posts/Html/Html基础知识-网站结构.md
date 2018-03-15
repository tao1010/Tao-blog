---
title: Html基础知识-网站结构
date: 2018-03-12 09:16:20
tags: html
---



一、文档的基本部分

一个典型的网站可能会这样布局：

![网站结构](网站结构.png)

1.标题: &lt;header&gt;

通常在顶部有一个大标题和（或）图标。 这是一个网站的主要常见信息，通常存在于每一个网页。
	
	<header>展现一系列介绍性内容。如果它是<body>的子元素，它就定义了网站的全局页眉,如果它是<article>或<section>的子元素,它就定义了这些部分的特定页眉。(不要与title and headings混淆)。
	
2.导航:&lt;nav&gt;

链接到网站的主要部分；通常由菜单按钮、链接或选项卡表示。与标题一样，这些内容通常在一个网页与另一个网页之间保持一致——在您的网站上导航不一致只会使人疑惑和恼火。许多网页设计师认为导航栏是标题的一部分，而不是独立的组件，但这并不是一个硬性规定；实际上有些人认为，两个独立的会有更好的可访问性，因为如果它们互相独立，屏幕阅读器可以更好地阅读两个功能。

	<nav>包含了页面主要的导航功能,二级链接等，不会进入导航功能部分。
	
3.主要内容:&lt;main&gt;具有代表性的内容段落主题可以使用&lt;article&gt;,&lt;section&gt;,&lt;div&gt;元素。

中心的一个大区域，包含给定网页的大部分独特内容，例如您要观看的视频，或您正在阅读的主要故事，或您要查看的地图，或新闻标题等......这是网站的一部分，绝对会因页面而异。

	<main>展现了页面内容的独特性。只可以在每一个页面上使用一次<main>,直接把它放到<body>中，在理想情况下，不应该把它嵌套进其他的元素中。
	
	<article>闭合一块与自身相关的内容，这块内容能够解释它自身而不是页面上其他的内容(eg:一篇单独的博客)。
	
	<section>近似于<article>，但是它更多的是伴随着一个单独功能构成的页面(eg:小型地图或一组文章的标题和摘要)。最好的实际应用是用标题作为每一部分(section)的开头，tips：可以把不同的<article>分到不同的<section>中，或者把不同的<section>分到不同的 <article>中，这取决于内容。

	有时候你可能只想仅仅用CSS或JavaScript将一组元素作为一个单独的实体来修饰。为了应对这种情况，HTML提供了<div>和<span>元素:
	
	<div>是一个块级无语义元素，仅仅当找不到更好的块级元素时使用，或者不想增加特定的意义。最好使用class属性来提供一些标签(eg:浏览电商网站，有一个购物车部件一直都可以选择的地方)。
	
	<span>是一个行内无语义元素，仅仅当无法找到更好的语义元素包含内容时使用，或者不想增加特定的含义(最好使用class属性来提供一些标签)。

	eg:
	<div class="shopping-cart">
	  <h2>Shopping cart</h2>
	  <ul>
	    <li>
	      <p><a href=""><strong>Silver earrings</strong></a>: $99.95.</p>
	      <img src="../products/3333-0985/" alt="Silver earrings">
	    </li>
	    <li>
	      ...
	    </li>
	  </ul>
	  <p>Total cost: $237.89</p>
	</div>
	
	这并不是一个<aside>,因为它和主要内容并没有必要的联系（你想在任何地方都能看到它）。它甚至不能用<section>来特定的指定，因为它不是页面上主要内容的一部分。所以在这种情况下用<div>是合适的。我们已经把它包含进head标签里作为帮助屏幕阅读者找到它的引导.
		
4.侧栏：&lt;aside&gt; ；经常嵌套在&lt;main&gt;中

一些次要信息、链接、引言、广告等。通常这是与主要内容中包含的内容相关（例如在新闻文章页面上，侧边栏可能包含作者的个人信息或相关文章的链接），但有在一些情况下，你会发现一些重复的元素，如辅助导航系统。

	<aside>包含了内容并不与主要内容有直接的联系，但是它可以提供额外的不直接由=有联系的信息(术语条目，作者简介，相关链接等) 。
	
5.页脚:&lt;footer&gt;

横跨页面底部的条纹，通常包含精美的打印、版权通知或联系信息。 它是一个放置公共信息（如标题）的地方，但通常该信息对网站来说不是特别重要。 通过提供用于快速访问热门内容的链接，页脚有时也用于SEO目的。

	<footer>包含了页面的页脚部分。

警告: div用起来非常便利以至于很容易被滥用。因为它们不携带语义值，所以会让你的HTML代码变的混乱。要小心的使用它们，只有当没有更好的语义解决方案才能使用，而且要尽可能把它的使用量降到最低，否则，当你升级和维护你的文档时会非常困难。

二、换行与水平分割线

1.&lt;br&gt;

在一个段落中创建一个换行；在你想要生成一系列的短行的地方，&lt;br&gt;是唯一能够生成这种结构的元素。

eg:一个邮寄地址或一首诗。

``` html
<p>There once was a girl called Nell<br>
Who loved to write HTML<br>
But her structure was bad, her semantics were sad<br>
and her markup didn't read very well.</p>
```

2.&lt;hr&gt;

&lt;hr&gt;元素在文档中生成一条水平分割线，表示文本中主题的变化(eg：主题或场景的变化)看起来就是一条水平线。

eg：

``` html
<p>Ron was backed into a corner by the marauding netherbeasts. Scared, but determined to protect his friends, he raised his wand and prepared to do battle, hoping that his distress call had made it through.</p>
<hr>
<p>Meanwhile, Harry was sitting at home, staring at his royalty statement and pondering when the next spin off series would come out, when an enchanted distress letter flew through his window and landed in his lap. He read it hasily, and lept to his feet; "better get back to work then", he mused.</p>
```

三、设计一个网站：

在一个结构庞大、复杂的网站里，大多数设计都可以参照上述的 information architecture（信息架构：一旦你设计好了一个简单网站的所有内容，按照正常的逻辑思维，我们应该尝试制定出你想要放在整个网站上的内容，哪些页面是你需要的，这些页面应该如何排列，以及如何互相链接，带给用户最好的使用体验。）

1.如果在你的设计中，每个页面都有一些内容是重复的，你可以先把这些重复的内容记录下来。

![1](1.png)

2.接下来，你可以通过画一个草图的方式来说明你希望的每个页面的结构的样子，（或许你画出来的草图和我们上文中提到的示例页面比较像），在空白段落上做上标记，来说明之后要填充在这里的内容。

![2](2.png)

3.现在，所有的网站设计人员可以一起讨论，还希望网站上显示哪些内容 （不包括每个页面的重复页面）— 以列表的形式写下来。

![3](3.png)

4.接着，尝试把这些内容进行分组，这样可以让你了解哪些内容可以放在一个相同的页面上。这种做法和 Card sorting 非常相似。

![4](4.png)

5.现在，尝试着再画一个网站的草图 — 每个气泡代表网站的一个页面，在气泡与气泡之间用连线的方式，来说明它们之间的联系。主页面可能位于中心位置，并且链接到其他的大多数页面；对于一个小型网站，大多数页面都可以从主页的导航栏中链接跳转，虽然也存在例外。你可能也希望记录下内容将如何显示的笔记。

![5](5.png)


参考资料：

1.[标题](https://developer.mozilla.org/en-US/Learn/HTML/Howto/Set_up_a_proper_title_hierarchy)

2.[title and headings](https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML/The_head_metadata_in_HTML#Adding_a_title)

3.[文件和网站结构](https://developer.mozilla.org/zh-CN/docs/learn/HTML/Introduction_to_HTML/文件和网站结构)