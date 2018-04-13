---
title: CSS-CSS网格布局
date: 2018-04-13 09:26:17
tags: CSS
categories: Web
toc: true
---

一、概念	
![grid](grid.png)	
1.网格由水平和垂直线集合创建的一个模式，根据这个模式排列设计元素。帮助我们创建设计——在页面之间移动时元素不会跳动或更改宽度，从而在我们的网站上提供高一致性。

2.网格通常具有列（column），行（row），以及在每行和列之间的间隙——通常称为沟槽（gutter）.		
二、创建简单的网格框架		
1.概述：	
大多数网格类型布局使用浮动创建的 - 创建多列布局等。    
要创建的最简单的网格框架类型是固定宽度的：	
	
	我们只需要计算出想要设计的总宽度，想要多少列，以及沟槽和列的宽度。
	如果我们决定在具有列根据浏览器宽度增长和缩小的网格上布置设计，我们需要计算出列和沟槽之间的百分比宽度。
2.一个简单的定宽网格		
![simple-grid-finished](simple-grid-finished.png)    
创建一个使用固定宽度的网格系统

```html
<div class="wrapper">
  <div class="row">
    <div class="col">1</div>
    <div class="col">2</div>
    <div class="col">3</div>
    <div class="col">4</div>
    <div class="col">5</div>
    <div class="col">6</div>
    <div class="col">7</div>
    <div class="col">8</div>
    <div class="col">9</div>
    <div class="col">10</div>
    <div class="col">11</div>
    <div class="col">12</div>
  </div>
  <div class="row">
    <div class="col span1">13</div>
    <div class="col span6">14</div>
    <div class="col span3">15</div>
    <div class="col span2">16</div>    
  </div>
</div>
```
```css
* {
  box-sizing: border-box;
}
    
//设置包装容器宽度980px;
body {
  width: 980px;
  margin: 0 auto;
}
//设置右侧边距20px;
.wrapper {
  padding-right: 20px;
}
解析：
	这使总共列/沟槽宽度960px，padding被整个content的宽度减去（所以元素的box-sizing：border-box）;
```
列之间的沟槽为20像素宽,每列的左侧创建一个20px的外边距（margin）作为沟槽——包括第一列，以平衡容器右侧的填充的20像素; 沟槽宽度：12 * 20 ，总宽度960，剩余每列宽度=（960-12*20)/12 = 60;

```css
.row{
	clear: both;
}
/**
在网格的每一行的行容器从另一行中清除一行，在上一个规则下面添加以下规则;
此清除意味着我们不需要应用构成完整十二列的元素去填充每一行。行将保持分离，并且彼此不干扰;
*/

.col{
	float: left;//向左浮动
	margin-left: 20px;//给它一个20像素的margin-left形成一个沟槽
	width: 60px;//一个60像素的 width
	background: rgb(255,150,150);//每列背景颜色
}
```
想要跨越多个列的布局容器需要被赋予特殊的类，来将它们的width 值调整到所需的列数（加上之间的沟槽）。我们需要创建一个额外的类，以允许容器跨越2到12列。每个宽度是将该列数的列宽加上沟槽宽度得到的结果，总是比列数少1：

```css
/* Two column widths (120px) plus one gutter width (20px) */
.col.span2 { width: 140px; }
/* Three column widths (180px) plus two gutter widths (40px) */
.col.span3 { width: 220px; }
/* And so on... */
.col.span4 { width: 300px; }
.col.span5 { width: 380px; }
.col.span6 { width: 460px; }
.col.span7 { width: 540px; }
.col.span8 { width: 620px; }
.col.span9 { width: 700px; }
.col.span10 { width: 780px; }
.col.span11 { width: 860px; }
.col.span12 { width: 940px; }
解析：
	设置每个跨度列所占有的宽度，eg:span7表示7个跨度占540px的宽度，每个元素根据自己的class渲染自己的宽度。
```
3.创建流体网格		
流体网格会随着浏览器视口的可用空间而相应变化，为了实现可以使用参考像素宽度并将其转化为百分比。   
将固定宽度变为基于百分比的灵活(flexible)宽度的公式：

	target / context = result
	eg:对于列宽，上下文是一个960px的容器，目标宽度是60px；
	60/960 = 0.0625 = 6.25%可以替换像素列宽度为60px的值。
更新网格

```css
body {
  width: 90%;
  max-width: 980px;//阻止布局变得太宽
  margin: 0 auto;
}

.wrapper {
  padding-right: 2.08333333%;
}

.col {
  float: left;
  margin-left: 2.08333333%;
  width: 6.25%;
  background: rgb(255, 150, 150);
}

/* Two column widths (12.5%) plus one gutter width (2.08333333%) */
.col.span2 { width: 14.58333333%; }
/* Three column widths (18.75%) plus two gutter widths (4.1666666) */
.col.span3 { width: 22.91666666%; }
/* And so on... */
.col.span4 { width: 31.24999999%; }
.col.span5 { width: 39.58333332%; }
.col.span6 { width: 47.91666665%; }
.col.span7 { width: 56.24999998%; }
.col.span8 { width: 64.58333331%; }
.col.span9 { width: 72.91666664%; }
.col.span10 { width: 81.24999997%; }
.col.span11 { width: 89.5833333%; }
.col.span12 { width: 97.91666663%; }
```
使用calc()函数更容易的计算，上述css等价于：

```css
.col.span2 { width: calc((6.25%*2) + 2.08333333%); }
.col.span3 { width: calc((6.25%*3) + (2.08333333%*2)); }
.col.span4 { width: calc((6.25%*4) + (2.08333333%*3)); }
.col.span5 { width: calc((6.25%*5) + (2.08333333%*4)); }
.col.span6 { width: calc((6.25%*6) + (2.08333333%*5)); }
.col.span7 { width: calc((6.25%*7) + (2.08333333%*6)); }
.col.span8 { width: calc((6.25%*8) + (2.08333333%*7)); }
.col.span9 { width: calc((6.25%*9) + (2.08333333%*8)); }
.col.span10 { width: calc((6.25%*10) + (2.08333333%*9)); }
.col.span11 { width: calc((6.25%*11) + (2.08333333%*10)); }
.col.span12 { width: calc((6.25%*12) + (2.08333333%*11)); }
```
4.语义和‘非语义’网格系统

```html
<div class="col span6">14</div>
```
```css
.col.span6 { width: 460px; }
或
.col.span6 { width: 47.91666665%; }
```
等价于：	

```html
<div class="contecnt">14</div>
```
```css
.content{
	width: calc((6.25%*6) + (2.08333333%*5));
}
```
5.在网格中启用偏移容器	
![offset-grid-finished](offset-grid-finished.png)    
eg:将容器元素偏移一列宽度

```html
将
<div class="col span6">14</div>
替换成：
<div class="col span5 offset-by-one">14</div>
```
```css
.offset-by-one {
  margin-left: calc(6.25% + (2.08333333%*2));
}
```
浮动网格限制		
当使用浮动网格时，需要注意：
	
	总宽度要加起来正确，并且不能在一行中包含跨（越）度为多列的超过该行所能包含的元素。由于浮动工作方式，如果网格列的数量相对于网格变得太宽，则末端上的元素将下降到下一行，从而打破网格。
	元素的内容比它们占据的行更宽，它会溢出，会看起来像一团糟。
这个系统的最大限制是它基本上是一维的。我们处理的是列元素只能跨越多个列，而不能跨越行。这些旧的布局方法非常难以控制元素的高度，而没有明确设置高度，这是一个非常不灵活的方法 - 它只有当你能保证你的内容将是一定的高度才有效。

三、Flexbox网格?

```css
body {
  width: 90%;
  max-width: 980px;
  margin: 0 auto;
}

.wrapper {
  padding-right: 2.08333333%;
}


.row {
  display: flex;
}

.col {
  margin-left: 2.08333333%;
  margin-bottom: 1em;
  width: 6.25%;
  flex: 1 1 auto;
  background: rgb(255,150,150);
}
```
1.Flexbox是一维设计。它处理单个维度，即行或列。不能为列和行创建严格的网格，这意味着如果我们要为网格使用flexbox，我们仍然需要为浮动布局计算百分比.

四、第三方网格系统			
流行框架：Bootstrap和Foundation

五、本地CSS网格和网格布局		
1.构建本地网络

```html
<div class="wrapper">
      <div class="col">1</div>
      <div class="col">2</div>
      <div class="col">3</div>
      <div class="col">4</div>
      <div class="col">5</div>
      <div class="col">6</div>
      <div class="col">7</div>
      <div class="col">8</div>
      <div class="col">9</div>
      <div class="col">10</div>
      <div class="col">11</div>
      <div class="col">12</div>
      <div class="col">13</div>
      <div class="col span6">14</div>
      <div class="col span3">15</div>
      <div class="col span2">16</div>     
    </div>
```
```css
.wrapper {
  width: 90%;
  max-width: 960px;
  margin: 0 auto;
  display: grid;
  grid-template-columns: repeat(12, 1fr);
  grid-gap: 20px;
}

.col {
  background: rgb(255,150,150);
}

.span2 { grid-column: auto / span 2;}
.span3 { grid-column: auto / span 3;}
.span4 { grid-column: auto / span 4;}
.span5 { grid-column: auto / span 5;}
.span6 { grid-column: auto / span 6;}
.span7 { grid-column: auto / span 7;}
.span8 { grid-column: auto / span 8;}
.span9 { grid-column: auto / span 9;}
.span10 { grid-column: auto / span 10;}
.span11 { grid-column: auto / span 11;}
.span12 { grid-column: auto / span 12;}

解析：
	可以使用属性的grid值声明一个网格，使用该display属性设置grid-gap一个网格；
	然后使用grid-template-columns新的repeat()函数和为网格布局定义的新单元创建一个12列相等宽度的网格 - fr单位；
	fr单位：它描述在网格容器的可用空间的一小部分。如果所有列都是1fr，它们将占用相等的空间量。这消除了计算百分比以创建灵活网格的需要
```

```css
跨6列
.span6 {
  grid-column: auto / span 6;
}

跨3列
.span3 {
  grid-column: auto / span 3;
}
```
CSS网格是二维的，因此随着布局的增长和缩小，元素保持水平和垂直排列。

```css
将
<div class="col">13</div>
<div class="col span6">14</div>
<div class="col span3">15</div>
<div class="col span2">16</div>
替换成：
<div class="col">13some<br>content</div>
<div class="col span6">14this<br>is<br>more<br>content</div>
<div class="col span3">15this<br>is<br>less</div>
<div class="col span2">16</div>
解析：
	有意添加了一些行break（<br>）标签，以强制某些列变得比其他列高。如果你尝试保存和刷新，你会看到列的高度调整为与最高的容器一样高，所以一切都保持整洁；
```
![css-grid-finished](css-grid-finished.png)    
2.CSS网格特性

```html
将:
<div class="col span2">16</div> 
修改:
<div class="col span2 content">16</div>
```
```css
将
.content{
	grid-colum: 2/8;
}
修改：
.content {
  grid-column: 2 / 8;
  grid-row: 3 / 5;
}
解析：
	将容器16，跨越行3-5，列2-8；
```
![css-grid-finished1](css-grid-finished1.png)    
不需要使用边距伪造沟槽或显式计算它们的宽度 — CSS网格具有这种功能内置的grid-gap属性;

六、例子  
![css-grid-finished2](css-grid-finished2.png)   

```html
<form>
    <p>First of all, tell us your name and age.</p>
      <div>
        <label for="fname">First name:</label>
        <input type="text" id="fname">
      </div>
      <div>
        <label for="lname">Last name:</label>
        <input type="text" id="lname">
      </div>
      <div>
        <label for="age">Age:</label>
        <input type="text" id="age">
      </div>
</form>
```
```css
html {
  font-family: sans-serif;
}
form {
  display: table;
  margin: 0 auto;
}
form div {
  display: table-row;
}
form label, form input {
  display: table-cell;
  margin-bottom: 10px;
}
form label {
  width: 200px;
  padding-right: 5%;
  text-align: right;
}
form input {
  width: 300px;
}
form p {
  display: table-caption;
  caption-side: bottom;
  width: 300px;
  color: #999;
  font-style: italic;
}
```  
修改如下：

```html
 <form>
	<p>First of all, tell us your name and age.</p>
	  
	    <label for="fname">First name:</label>
	    <input type="text" id="fname">
	  
	  
	    <label for="lname">Last name:</label>
	    <input type="text" id="lname">
	  
	  
	    <label for="age">Age:</label>
	    <input type="text" id="age">

</form>
```
```css
html {
  font-family: sans-serif;
}
form {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  grid-gap: 10px 5%;
  width: 508px;
  margin: 0 auto;
}
form label {
  grid-column: auto / span 2;
  text-align: right;
}
form input {
  width: 100%;
  grid-column: auto / span 3;
}
form p {
  grid-column: auto / span 5;
  grid-row: 4;
  color: #999;
  font-style: italic;
}
```
参考资料：	
1.[网格](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/CSS_layout/Grids)		
2.[calc()函数](https://developer.mozilla.org/zh-CN/docs/Web/CSS/calc())
