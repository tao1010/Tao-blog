---
title: CSS-CSS定位
date: 2018-04-10 16:33:49
tags: CSS
categories: Web
toc: true
---

定位允许从正常的文档流布局中取出元素，并使他们具有不同的行为

一、文档流

1.盒模型：

	环绕元素内容添加任何内边距、边界和外边距来布置单个元素盒子。

2.块级元素：

	默认情况下，内容宽度是其父元素的宽度的100％，并且与其内容一样高；
	在视口中垂直布局——每个都将显示在上一个元素下面的新行上，并且它们的外边距将分隔开它们；
	
3.内联元素：

	高宽与他们的内容高宽一样。不能对内联元素设置宽度或高度——它们只是位于块级元素的内容中。 
	如果要以这种方式控制内联元素的大小，则需要将其设置为类似块级元素 display: block;
	不会出现在新行上；相反，它们互相之间以及任何相邻（或被包裹）的文本内容位于同一行上，只要在父块级元素的宽度内有空间可以这样做。如果没有空间，那么溢流的文本或元素将向下移动到新行；
	
4.外边距折叠：

	如果两个相邻元素都在其上设置外边距，并且两个外边距接触，则两个外边距中的较大者保留，较小的一个消失；

```html
<body>
  <h1>Basic document flow</h1>

  <p>I am a basic block level element. My adjacent block level elements sit on new lines below me.</p>
  
  <p>By default we span 100% of the width of our parent element, and we are as tall as our child content. Our total width and height is our content + padding + border width/height.</p>
  
  <p>We are separated by our margins. Because of margin collapsing, we are separated by the width of one of our margins, not both.</p>
  
  <p>inline elements <span>like this one</span> and <span>this one</span> sit on the same line as one another, and adjacent text nodes, if there is space on the same line. Overflowing inline elements will <span>wrap onto a new line if possible (like this one containing text)</span>, or just go on to a new line if not, much like this image will do: <img src="https://mdn.mozillademos.org/files/13360/long.jpg"></p>
</body>
```

```css
body {
  width: 500px;
  margin: 0 auto;
}

p {
  background: aqua;
  border: 3px solid blue;
  padding: 10px;
  margin: 10px;
}

span {
  background: red;
  border: 1px solid black;
}

```
![文档流](float-position.png)   
二、介绍定位-position  
1.静态定位			
将元素放入它在文档布局流中正常位置即每个元素默认值。

```html					
<p class="positioned">are separated by our margins. Because of margin </p>
```
```css
.positioned{

  position: static;
  background: yellow;
}
```
2.相对定位			
与静态定位类似，占据在正常的文档流中，除非你修改他的位置（top,bottom,left,right）

```css	
.positioned{

  position: relative;
  background: yellow;
}
```
tips:修改后界面不会有变化		
3.介绍top,bottom,left,right		
用来精确定位元素移动到的位置，这些属性值可以采用任何单位(px,mn,rems,%等)

```css
.positioned{

  position: relative;//相对定位会以上一个相邻元素作为参照物
  background: yellow;
  top: 5rem;//距离顶部5rem距离
  left: 5rem;//距离左侧5rem距离
}
```
tips:修改元素位置在static静态定位下无效，在相对定位和绝对定位下有效		
4.绝对定位		
绝对定位的元素不再存在于正常文档布局流程中，它自己的层独立于一切，这样可以创建不干扰其他元素的位置的隔离UI功能如弹出框，控制菜单，翻转面板，可以在页面上任何地方拖放UI功能

```css
.positioned{

  position: absolute;//绝对定位会以页面的左上角为起点作为参照物
  background: yellow;
  top: 1rem;
  left: 5rem;
}
```
eg:此种情况定位的元素会铺满整个页面

```css
.positioned{

  position: absolute;
  background: yellow;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  margin: 0;
}
```
tips:外边距会影响定位的元素，外边距折叠不会。	
5.定位上下文		
元素的定位上下文:默认情况下，它是<html>元素——定位的元素是被嵌套在<body>中的HTML源代码中被position修饰的元素。

6.介绍z-index
多个元素定位

```html
<body>
  <h1>Basic document flow</h1>

  <p>I am a basic block level element. My adjacent block level elements sit on new lines below me.</p>
  
  <p>By default we span 100% of the width of our parent element, and we are as tall as our child content. Our total width and height is our content + padding + border width/height.</p>
  
  <p>We are separated by our margins. Because of margin collapsing, we are separated by the width of one of our margins, not both.</p>
  
  <p>inline elements <span>like this one</span> and <span>this one</span> sit on the same line as one another, and adjacent text nodes, if there is space on the same line. Overflowing inline elements will <span>wrap onto a new line if possible (like this one containing text)</span>, or just go on to a new line if not, much like this image will do: <img src="https://mdn.mozillademos.org/files/13360/long.jpg"></p>

  <p class="positioned">are separated by our margins. Because of margin </p>

</body>
```
```css
.positioned{

  position: absolute;
  background: yellow;
  top: 30px;
  left: 30px;
  /* bottom: 50%; */
  /* right: 80%;
  margin: 0; */
}
p:nth-of-type(2){

  position: absolute;
  background: limegreen;
  top: 40px;
  right: 30px;
}
```
![定位上下文](position-context.png)		
解析：
	
	p:nth-of-type(2)修饰的一段为绿色，移除文档流程，并于原始位置上方一点；
	.positioned修饰的一段与p:nth-of-type(2)修饰的一段重叠，但是p:nth-of-type(2)修饰的一段在html源顺序中第一段，因此先渲染，其后渲染的.positioned修饰的一段将放在最顶上；
若要更改堆叠顺序,可以使用z-index属性, “z-index”是对z轴的参考,网页也有一个z轴——一条从屏幕表面到你的脸（或者在屏幕前面你喜欢的任何其他东西）的虚线。z-index 值影响定位元素位于该轴上的位置——正值将它们移动到堆栈上方，负值将它们向下移动到堆栈中。默认情况下，定位的元素都具有z-index为auto，实际上为0;

``` css
p:nth-of-type(2){

  position: absolute;
  background: limegreen;
  top: 40px;
  right: 30px;
  z-index: 1;
}
```
![z-index](position-context-z-index.png)	
tips：z-index只接受无单位索引值；较高的值将高于较低的值，这取决于您使用的值。 使用2和3将产生与300和40000相同的效果。	
7.固定定位		
固定定位-定位覆盖fixed与绝对定位的工作方式完全相同，主要区别：绝对定位固定元素是相对于&lt;html&gt;元素或其最近的定位祖先，固定定位是相对于浏览器视口本身，因此可以创建固定的UI如持久导航菜单。

```html
<body>
  <h1>Basic document flow</h1>

  <p>I am a basic block level element. My adjacent block level elements sit on new lines below me.</p>
  
  <p>By default we span 100% of the width of our parent element, and we are as tall as our child content. Our total width and height is our content + padding + border width/height.</p>
  
  <p>We are separated by our margins. Because of margin collapsing, we are separated by the width of one of our margins, not both.</p>
  
  <p>inline elements <span>like this one</span> and <span>this one</span> sit on the same line as one another, and adjacent text nodes, if there is space on the same line. Overflowing inline elements will <span>wrap onto a new line if possible (like this one containing text)</span>, or just go on to a new line if not, much like this image will do: <img src="https://mdn.mozillademos.org/files/13360/long.jpg"></p>

  <p class="positioned">are separated by our margins. Because of margin </p>

</body>
```
```css
body {
  width: 500px;
  height: 1400px;
  margin: 0 auto;
}

p {
  background: aqua;
  border: 3px solid blue;
  padding: 10px;
  margin: 10px;
}

span {
  background: red;
  border: 1px solid black;
}

h1{
 //给<h1>元素 position: fixed;，并让它坐在视口的顶部中心
  position: fixed;
  top: 0;
  width: 500px;
  margin: 0 auto;
  background: white;
  padding: 10px;
}

p:nth-of-type(1){

  margin-top: 60px;
}
```
解析：

	 top: 0;是要使它贴在屏幕的顶部；我们然后给出标题与内容列相同的宽度，并使用可靠的老技巧 margin: 0 auto; 使它居中。 然后我们给它一个白色背景和一些内边距，所以内容将不会在它下面可见。

	标题保持固定，内容显示向上滚动并消失在其下。 但是我们可以改进这一点——目前标题下面挡住一些内容的开头。这是因为定位的标题不再出现在文档流中，所以其他内容向上移动到顶部。 在第一段设置一些顶部边距来做到把它向下移动一点。
![标题滚动](position-context-1.png)

8.实验属性:position sticky

有一个新的定位值可用称为 position: sticky，尚未广泛支持。 这基本上是相对位置和固定位置之间的混合，其允许定位的元件像它被相对定位一样动作，直到其滚动到某一阈值点（例如，从视口顶部10像素），之后它变得固定。阅读[position:sticky 引用入口](https://developer.mozilla.org/zh-CN/docs/Web/CSS/position#Sticky_positioning)。

参考资料：   
1.[CSS定位](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/CSS_layout/定位)
