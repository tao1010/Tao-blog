---
title: CSS-样式框-高级盒子效果
date: 2018-03-26 14:11:26
tags: CSS
---

一、盒子阴影   
box-shadow 允许将一个或多个阴影应用到一个实际的元素盒子中;
text-shadow 允许将一个或多个阴影应用到元素的文本上;   

```html
HTML:
<article class="simple">
  <p><strong>Warning</strong>: The thermostat on the cosmic transcender has reached a critical level.</p>
</article>
```
1.一个盒子阴影

```css
CSS:
p {
  margin: 0;
}

article {
  max-width: 500px;
  padding: 10px;
  background-color: red;
  background-image: linear-gradient(to bottom, rgba(0,0,0,0), rgba(0,0,0,0.25));
}  

.simple {
  box-shadow: 5px 5px 5px rgba(0,0,0,0.7);
}
```
2.多个盒子阴影

在单个box-shadow声明中指定多个框阴影，用逗号分隔：

```css
CSS:

... 

.simple {
  box-shadow: 5px 5px 5px rgba(0,0,0,0.7),
  				 1px 1px 1px black,
              2px 2px 1px black,
              3px 3px 1px red,
              4px 4px 1px red,
              5px 5px 1px black,
              6px 6px 1px black;
}
```
3.解析:在box-shadow属性值中有4个项：

	box-shadow: 5px 5px 5px rgba(0,0,0,0.7);
	
	第一个长度值是水平偏移量（horizontal offset ）——即向右的距离，阴影被从原始的框中偏移(如果值为负的话则为左)。
	第二个长度值是垂直偏移量（vertical offset）——即阴影从原始盒子中向下偏移的距离(或向上，如果值为负)。
	第三个长度的值是模糊半径（blur radius）——在阴影中应用的模糊度。
	颜色值是阴影的基本颜色（base color）
可以使用任何长度和颜色单位来定义。

4.其他盒子阴影特点    
与text-shadow不同，box-shadow有一个inset关键字可用——把它放在一个影子声明的开始，使它变成一个内部阴影，而不是一个外部阴影。   
eg:

```html
HTML:
<button>Press me!</button>
```
```css
CSS
button {
  width: 150px;
  font-size: 1.1rem;
  line-height: 2;
  border-radius: 10px;
  border: none;
  background-image: linear-gradient(to bottom right, #777, #ddd);
  box-shadow: 1px 1px 1px black,
              inset 2px 3px 5px rgba(0,0,0,0.3),
              inset -2px -3px 5px rgba(255,255,255,0.5);
}

button:focus, button:hover {
  background-image: linear-gradient(to bottom right, #888, #eee);
}

button:active {
  box-shadow: inset 2px 2px 1px black,
              inset 2px 3px 5px rgba(0,0,0,0.3),
              inset -2px -3px 5px rgba(255,255,255,0.5);
}
```
解析：   

	将focus/hover/active这些声明一起设置了按钮样式；
	按钮的默认状态下设置了一个简单的黑色盒阴影，并且加上了一对inset阴影，一个明的，一个暗的，位于按钮的两个对角上，以此给按钮一种很棒的阴影效果；
	当按钮被按下时，这里的active声明将第一个盒阴影换成一个非常暗的inset阴影。给人一种按钮被按下的样子。
tips:

	可以在box-shadow中设置的值 — 另外一个位于颜色值前面可选的长度值，即spread radius，如果设置了这个值，将会导致阴影变得比原始的阴影更大，这个值不是很常用.	
![box-shadow](box-shadow.png)   
二、过滤器(Filters)

1.使用

	基本上，过滤器可以应用在任何元素上，块元素（block）或者行内元素（inline）——你只需要使用filter属性，并且给他一个特定的过滤函数的值。
	有些可用的过滤器选项和其他CSS特性做的事情十分相似，例如drop-shadow()的工作方式以及产生的效果和 box-shadow 或text-shadow十分相似。
	然而过滤器真正出色的地方在于，它们作用于盒（box）内内容（content）的确切形状，而不仅仅将盒子本身作为一个大的块，这看起来会更棒，即使他们可能不会总是变成你想要的模样。
```html
HTML
<p class="filter">Filter</p>
<p class="box-shadow"> Box shadow</p>
```

```css
CSS
/* 过滤器 */
p{
  margin: 1rem auto;
  padding: 20px;
  width: 100px;
  border: 5px dashed red;
}

.filter{
  -webkit-filter: drop-shadow(5px 5px 1px rgba(0,0,0,0.7));
  filter: drop-shadow(5px 5px 1px rgba(0,0,0,0.7));
}
.box-shadow{

  box-shadow: 5px 5px 1px rgba(0,0,0,0.7);
}
```
![filter](filter.png)   
解析:drop-shadow过滤器跟随着文本和border dashes的确切形状。而盒阴影（box-shadow）仅仅跟随着盒的四方

	过滤器很新——被大多数的现代的浏览器支持，包括Microsoft Edge，但不能被IE浏览器支持.
	在filter属性中通过-webkit-前缀包含了一个版本信息，这被称为一个 Vendor Prefix，有时会被浏览器使用，以在一个新特性完整实现之前，当它与无前缀版本没有冲突的时候支持并实验这个特性。当实验性的特性确实被需要时使用。Chrome/Safari/Opera 目前要求这些属性的-webkit-版本，而Edge 和 Firefox则使用后者，无前缀版本;
	
	如果你确实决定在你的代码中使用前缀，确保你包括了所有需要的前缀以及无前缀的版本，这样才会有尽可能多的浏览器能够使用这些特性，并且如果浏览器落下了前缀，它们也能够使用无前缀的版本。
	另外需要注意的是这些实验性的特性可能会有改变，这可能会导致你的代码被破坏，在前缀被去除之前，最好还是仅仅实验这些特性;
三、Blend modes(混合模式)   
1.混合模式概述	

	CSS混合模式允许我们为元素添加一个混合模式 ，以当两个元素重叠时，指定一个混合的效果——最终每个像素所展示的颜色将会是原来像素中颜色和其下面一层相组合之后的结果;
2.属性
	
	background-blend-mode, 用来将单个元素的多重背景图片和背景颜色设置混合在一起;
	mix-blend-mode, 用来将一个元素与它覆盖的那些元素各自所设置的背景（background）和内容(content)混合在一起;

3.浏览器兼容性

	 混合模式（Blend modes ）同样也很新，而且略微不如过滤器（filter）的被支持度。至今也没有没Edge支持， 并且 Safari 也仅仅支持部分混合模式选项;

4.background-blend-mode

```html
HTML
<!-- 混合模式 -->
<div></div>
<div class="multiply"></div>
```
```css
CSS
div {
  width: 280px;
  height: 130px;
  padding: 10px;
  margin: 10px;
  display: inline-block;
  background: url(https://mdn.mozillademos.org/files/13090/colorful-heart.png) no-repeat center 20px;
  background-color: green;
}

.multiply {
  background-blend-mode: multiply;
}
```
![background-blend-mode](background-blend-mode.png)

	左边两张图为原图，右边图为多层混合版本
5.mix-blend-mode	

```html
HTML
<article>
  No mix blend mode
  <div>
        
  </div>
  <div>
  </div>
</article>

<article>
  Multiply mix
  <div class="multiply-mix">
        
  </div>
  <div>
  </div>
</article>
```
```css
CSS
article {
  width: 300px;
  height: 180px;
  margin: 10px;
  position: relative;
  display: inline-block;
}

div {
  width: 250px;
  height: 130px;
  padding: 10px;
  margin: 10px;
}

article div:first-child {
  position: absolute;
  top: 10px;
  left: 0;
  background: url(https://mdn.mozillademos.org/files/13090/colorful-heart.png) no-repeat center 20px;
  background-color: green;
}

article div:last-child {
  background-color: purple;
  position: absolute;
  bottom: -10px;
  right: 0;
  z-index: -1;
}

.multiply-mix {
  mix-blend-mode: multiply;
}
```
![mix-blend-mode](mix-blend-mode.png)

	多层混合（mix-blend）不仅混合了两种背景图像，还混合了在&lt;div&gt;下面的颜色。
6.-webkit-background-clip: text

	(目前支持Chrome、Safari和Opera,和在Firefox正在实现)是background-clip的 text 值。
	当与专有 -webkit-text-fill-color: transparent; 特性一起使用时，这允许您将背景图像剪贴到元素文本的形状，从而产生一些不错的效果。
```css
.text-clip {
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}
```
为什么使用-webkit-前缀：

	主要是为了浏览器兼容性——许多web开发人员已经开始使用-webkit-前缀来实现web站点，它开始看起来像其他的浏览器一样被破坏了，而实际上他们遵循的是标准。

参考资料：   
1.[盒子效果](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Styling_boxes/Advanced_box_effects)
