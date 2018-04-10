---
title: CSS-CSS布局
date: 2018-03-27 13:58:51
tags: CSS
categories: Web

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
float属性最初指用于在成块的文本内浮动图像，现在已成为网页上创建多列布局的最常用工具之一。

1.简单实例 - 图片浮动

``` html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <link rel="stylesheet" href="css-floats.css">
</head>
<body>
  
  <view>
    <h1>Simple float example</h1>

    <img src="https://mdn.mozillademos.org/files/13340/butterfly.jpg" alt="A pretty butterfly with red, white, and brown coloring, sitting on a large leaf">

    <p> Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla luctus aliquam dolor, eu lacinia lorem placerat vulputate. Duis felis orci, pulvinar id metus ut, rutrum luctus orci. Cras porttitor imperdiet nunc, at ultricies tellus laoreet sit amet. Sed auctor cursus massa at porta. Integer ligula ipsum, tristique sit amet orci vel, viverra egestas ligula. Curabitur vehicula tellus neque, ac ornare ex malesuada et. In vitae convallis lacus. Aliquam erat volutpat. Suspendisse ac imperdiet turpis. Aenean finibus sollicitudin eros pharetra congue. Duis ornare egestas augue ut luctus. Proin blandit quam nec lacus varius commodo et a urna. Ut id ornare felis, eget fermentum sapien.</p>

    <p>Nam vulputate diam nec tempor bibendum. Donec luctus augue eget malesuada ultrices. Phasellus turpis est, posuere sit amet dapibus ut, facilisis sed est. Nam id risus quis ante semper consectetur eget aliquam lorem. Vivamus tristique elit dolor, sed pretium metus suscipit vel. Mauris ultricies lectus sed lobortis finibus. Vivamus eu urna eget velit cursus viverra quis vestibulum sem. Aliquam tincidunt eget purus in interdum. Cum sociis natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus.</p>

  </view>

  <script src="css-floats.js"></script>
</body>
</html>
```	
```css
body{

    width: 90%;
    max-width: 900px;
    margin: 0 auto;
}
p{
    line-height: 2;
    word-spacing: 0.1rem;
}
img{

    float: left;
    margin-right: 30px;

    /* float: right;
    margin-left: 30px; */

    /* float: center;
    margin-right: 30px; */
    /* margin-left: 30px; */
}
```
![浮动图片居左](float-left.png)   
解析:

	浮动元素img脱离正常的文档布局中,吸附到父容器view的左侧，在正常布局中位于该浮动元素之下的内容，会围绕着浮动元素，填满其右侧；
	浮动的内容任然遵循盒子模型（外边距、边界），设置外边距就能阻止右侧的文字紧贴图片；
2.简单实例 - 首字下沉

```html
<view>

    <p>This is my very important paragraph.
      I am a distinguished gentleman of such renown that my paragraph
      needs to be styled in a manner befitting my majesty. Bow before
     my splendour, dear students, and go forth and learn CSS!</p>
  </view>
```	

```css
/* 首字下沉 */
p{
    width: 400px;
    margin: 0 auto;
}
p::first-line{

    text-transform: uppercase;
}
p::first-letter{
    font-size: 3em;
    border: 1px solid black;
    background: red;
    float: left;
    padding: 2px;
    margin-right: 4px;
}

```
![首字下沉](float-first-down.png)    
3.多列浮动布局    
两列布局

```html
<!-- 两列布局 -->
  <view>

    <h1>2 column layout example</h1>
    <div> <!--采用div外部元素来包含内容，也可以用article,section,aside等 -->
      <h2>First column</h2>
      <p> Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla luctus aliquam dolor, eu lacinia lorem placerat vulputate. Duis felis orci, pulvinar id metus ut, rutrum luctus orci. Cras porttitor imperdiet nunc, at ultricies tellus laoreet sit amet. Sed auctor cursus massa at porta. Integer ligula ipsum, tristique sit amet orci vel, viverra egestas ligula. Curabitur vehicula tellus neque, ac ornare ex malesuada et. In vitae convallis lacus. Aliquam erat volutpat. Suspendisse ac imperdiet turpis. Aenean finibus sollicitudin eros pharetra congue. Duis ornare egestas augue ut luctus. Proin blandit quam nec lacus varius commodo et a urna. Ut id ornare felis, eget fermentum sapien.</p>
    </div>

    <div>
      <h2>Second column</h2>
      <p>Nam vulputate diam nec tempor bibendum. Donec luctus augue eget malesuada ultrices. Phasellus turpis est, posuere sit amet dapibus ut, facilisis sed est. Nam id risus quis ante semper consectetur eget aliquam lorem. Vivamus tristique elit dolor, sed pretium metus suscipit vel. Mauris ultricies lectus sed lobortis finibus. Vivamus eu urna eget velit cursus viverra quis vestibulum sem. Aliquam tincidunt eget purus in interdum. Cum sociis natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus.</p>
    </div>
  </view>
``` 
```css
/* 两列布局 */
body{
	在宽度达到900px之前，整个视图的宽度将达到90%，在超过900px后，它将保持在这个宽度，并在视口中居中。
    width: 90%;
    max-width: 900px;
    margin: 0 auto;
}
div:nth-of-type(1){
    width: 48%;
    float: left;
}
div:nth-of-type(2){

    width: 48%;
    float: right;
}
```		
![两列布局](float-two-col-layout.png)	   
三列布局

``` html
  <view>

    <h1>3 column layout example</h1>
    <div> 
      <h2>First column</h2>
      <p> Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla luctus aliquam dolor, eu lacinia lorem placerat vulputate. Duis felis orci, pulvinar id metus ut, rutrum luctus orci. Cras porttitor imperdiet nunc, at ultricies tellus laoreet sit amet. Sed auctor cursus massa at porta. Integer ligula ipsum, tristique sit amet orci vel, viverra egestas ligula. Curabitur vehicula tellus neque, ac ornare ex malesuada et. In vitae convallis lacus. Aliquam erat volutpat. Suspendisse ac imperdiet turpis. Aenean finibus sollicitudin eros pharetra congue. Duis ornare egestas augue ut luctus. Proin blandit quam nec lacus varius commodo et a urna. Ut id ornare felis, eget fermentum sapien.</p>
    </div>

    <div>
      <h2>Second column</h2>
      <p>Nam vulputate diam nec tempor bibendum. Donec luctus augue eget malesuada ultrices. Phasellus turpis est, posuere sit amet dapibus ut, facilisis sed est. Nam id risus quis ante semper consectetur eget aliquam lorem. Vivamus tristique elit dolor, sed pretium metus suscipit vel. Mauris ultricies lectus sed lobortis finibus. Vivamus eu urna eget velit cursus viverra quis vestibulum sem. Aliquam tincidunt eget purus in interdum. Cum sociis natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus.</p>
    </div>
    <!-- 三列布局 -->
    <div>
      <h2>Third column</h2>
      <p>Nam consequat scelerisque mattis. Duis pulvinar dapibus magna, eget congue purus mollis sit amet. Sed euismod lacus sit amet ex tempus, a semper felis ultrices. Maecenas a efficitur metus. Nullam tempus pharetra pharetra. Morbi in leo mauris. Nullam gravida ligula eros, lacinia sagittis lorem fermentum ut. Praesent dapibus eros vel mi pretium, nec convallis nibh blandit. Sed scelerisque justo ac ligula mollis laoreet. In mattis, risus et porta scelerisque, augue neque hendrerit orci, sit amet imperdiet risus neque vitae lectus. In tempus lectus a quam posuere vestibulum. Duis quis finibus mi. Nullam commodo mi in enim maximus fermentum. Mauris finibus at lorem vel sollicitudin.</p>
    </div>

  </view>
```
```css
/* 三列布局 */
body{
    width: 90%;
    max-width: 900px;
    margin: 0 auto;
}
div:nth-of-type(1){
    width: 36%;
    float: left;
}
div:nth-of-type(2){

    width: 20%;
    float: left;
    margin-left: 4%;
}

div:nth-of-type(3){

    width: 26%;
    float: right;
}
```
![三列布局](float-three-col-layout.png)   
4.清除浮动

前文用了浮动，之后的内容想保留在指定位置比如：footer包含的元素内容显示在页脚底部

```html
<footer>
        <p>&copy;2016 your imagination. This isn't really copyright, this is a mockery of the very concept. Use as you wish.</p>
      </footer>
```
```css
footer{
	clear: both;
}
```
解析:

	将clear属性应用到元素上，是的这个元素和源码后面的将不浮动，除非将一个新的float声明到另外的元素上；
	clear 可以取三个值：
		left：停止任何活动的左浮动
		right：停止任何活动的右浮动
		both：停止任何活动的左右浮动

5.浮动问题
整个宽度可能难以计算    
	
	当加上样式框时(背景、外边距、内边距等),问题出现宽度难以计算，一行可能容不下三列；
		解决方案：
		*{
			box-sizing: border-box;
			
		}
		通过更改盒模型来拯救我们，盒子的宽度取值为 content + padding + border，而不仅是之前的content——所以当增加内边距或边界的宽度时，不会使盒子更宽——而是会使内容调整得更窄。
	页脚压在最长列上：
		footer {
		  clear: both;
		  margin-top: 4%;
		}
		此语法无效：
			首先，他们在父元素中所占的面积的有效高度为0，用开发工具查看 <body> 的高度，正文高度只有 <h1> 的高度 。
			这个可以通过很多方式解决，但是我们所依赖的是在父容器的底部清除浮动，如我们在我们的当前示例所做的那样。 如果检查当前示例中正文的高度，您应该看它的高度是行为本身。
			其次，非浮动元素的外边距不能用于它们和浮动元素之间来创建空间；
	解决这个！ 首先，在HTML的代码里添加新的<div> 元素，位于在<footer>标签的上方：
		<div class="clearfix"></div>
		
		.clearfix {
			  clear: both;
		}
	
浮动项目的背景高度   
	
	多列布局的时候，列的高度可能不同，影响视觉效果，可以通过设置所有列的固定高度解决问题；
		.colum{
			height:550px;
		}
	将这些列的背景颜色设置为父元素的背景颜色，这样您就不会看到高度是不同的。这是目前最好的选择。
	将它们设置为固定的高度，并使内容滚动overflow (参见我们溢流部分的示例。)
	使用一种叫做伪列（faux columns）的技术——这包括将背景(和边界)从实际的列中提取出来，并在列的父元素上画一个伪造的背景，看起来像列的背景一样。不幸的是，这将无法处理列边界。
清除浮动会变复杂   

	当布局变得复杂，清理浮动也会复杂;
	需要确保所有浮动尽快清除,避免给下方的内容再次麻烦;
	没有方便的容器进行清理,必要时使用clearfix块。
参考资料：   
1.[布局回顾](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/CSS_layout/Introduction)   
2.[布局](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/CSS_layout)   
3.[浮动](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/CSS_layout/Floats)    
4.[关于float](https://css-tricks.com/all-about-floats/)   