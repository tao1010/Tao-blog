---
title: CSS-CSS布局
date: 2018-03-27 13:58:51
tags: CSS
---
一、CSS布局回顾

1.布局正常流   
指在不对页面进行任何布局控制时，浏览器默认的HTML布局方式。布局技术会覆盖默认的布局行为：

	position 属性 — 正常布局流中，默认为 static ，使用其它值会引起元素不同的布局方式，例如将元素固定到浏览器视口的左上角。
	浮动——应用 float 值，诸如 left 能够让块级元素互相并排成一行，而不是一个堆叠在另一个上面。
 	display 属性——标准值 block, inline or inline-block 会改变元素在正常布局流中的行为方式(见 Types of CSS boxes )，而一些不常见或特殊的值允许我们使用完全不同的方式进行布局，使用工具比如Flexbox。
2.浮动   
浮动技术允许元素浮动到另外一个元素的左侧或右侧，而不是默认的一个堆叠另一个。float 的主要用途是布置出多个列并且浮动文字以环绕图片，float属性：
	
	left — 将元素浮动到左侧。
	right — 将元素浮动到右侧。
	none — 默认值, 不浮动。
	inherit — 继承父元素的浮动属性。
2.1默认排版	

``` htmml
<h1>2 column layout example</h1>

<div>
  <h2>First column</h2>
  <p> Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla luctus aliquam dolor, eu lacinia lorem placerat vulputate. </p>
</div>

<div>
  <h2>Second column</h2>
  <p>Nam vulputate diam nec tempor bibendum. Donec luctus augue eget malesuada ultrices. Phasellus turpis est, posuere sit amet dapibus ut.</p>
</div>
```
![defalut](defalut.png)  
2.2整列浮动

```css
div:nth-of-type(1){

    width: 48%;
    float: left;
}
div:nth-of-type(2){

    width: 48%;
    float: right;
}
```
![float](float.png)   
3.定位技术    
定位技术(position)允许我们将一个元素从它在页面的原始位置准确地移动到另外一个位置。有四种主要的定位类型：

	静态定位(Static positioning)是每个元素默认的属性——它表示“将元素放在文档布局流的默认位置——没有什么特殊的地方”。
	相对定位(Relative positioning)允许我们相对元素在正常的文档流中的位置移动它——包括将两个元素叠放在页面上。这对于微调和精准设计(design pinpointing)非常有用。
	绝对定位(Absolute positioning)将元素完全从页面的正常布局流中移出，类似将它单独放在一个图层中. 我们可以将元素相对于页面的 <html> 元素边缘固定，或者相对于离元素最近的被定位的祖先元素(ancestor element)。绝对定位在创建复杂布局效果时非常有用，例如通过标签显示和隐藏的内容面板或者通过按钮控制滑动到屏幕中的信息面板.
	固定定位(Fixed positioning)与绝对定位非常类似，除了它是将一个元素相对浏览器视口固定，而不是相对另外一个元素。 在创建类似页面滚动总是处于页面上方的导航菜单时非常有用。

```html
<h1>Positioning</h1>

<p>I am a basic block level element.</p>
<p class="positioned">I am a basic block level element.</p>
<p>I am a basic block level element.</p>
```
```css
/* 定位 */
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


.positioned{

	position: relative;  /* 相对定位 */
	position: absolute;/* 绝对定位 */
	
	background: yellow;
	top: 30px;
	left: 30px;
}
```
![position](position.jpg)  
作用：
	
	相对定位通常用来对布局进行微调，比如将一个图标往下调一点，以便放置文字；
	绝对定位用于将元素移动到web页面的任何位置，以创建复杂的布局。有趣的是，它经常被用于与相对定位和浮动的协同工作；
	top和left属性对绝对位置元素的影响不同于相对位置元素。在这种情况下，没有指定元素相对于原始位置的移动程度。相反，它们指定元素应该从页面边界的顶部和左边的距离(确切地说，是 <html>元素的距离)。
	暂时不讨论固定定位（ fixed positioning ）——它基本上以相同的方式工作，除了它仍然固定在浏览器窗口的边缘，而不是它定位的父节点的边缘。
4.CSS表格
   
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
详情见下章   
5.柔性盒子

```html
<section>
  <div>This is a box</div>
  <div>This is a box</div>
  <div>This is a box</div>
</section>

<button class="create">Create box</button>
<button class="reset">Reset demo</button>
<script>
  var section = document.querySelector('section');
  var createBtn = document.querySelector('.create');
  var resetBtn = document.querySelector('.reset');
  function createBox() {
    var box = document.createElement('div');
    box.textContent = 'This is a box';
    section.appendChild(box);
  }
  createBtn.onclick = createBox;
  resetBtn.onclick = function() {
    while (section.firstChild) {
        section.removeChild(section.firstChild);
    }
    createBox();
    createBox();
    createBox();
  }
</script>
```
```css
/* 柔性盒子 */
  section {
    width: 93%;
    height: 240px;
    margin: 20px auto;
    background: purple;
    display: flex;
  }
  
  div {
    color: white;
    background: orange;
    flex: 1;
    margin-right: 10px;
    text-shadow: 1px 1px 1px black;
  }
  
  div:last-child {
    margin-right: 0;
  }
  
  section, div {
    border: 5px solid rgba(0,0,0,0.85);
    padding: 10px;
  }
```
CSS解析:
	
	display: flex; 告诉<section>元素的子元素作为flexible boxes——默认情况下，它们都将展开以填充父类的可用高度，不管它是什么，并将其列出来——有足够的宽度来包装他们的内容。
	flex: 1; 告诉每个<div>元素，在行中获得相同数量的空间，不管有多少;
	添加一些JavaScript，以便您可以通过按下Create box按钮来添加进一步的子 <div>;

6.网格布局

二、浮动






参考资料：   
1.[布局回顾](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/CSS_layout/Introduction)   
2.[布局](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/CSS_layout)   
3.[浮动](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/CSS_layout/Floats)    