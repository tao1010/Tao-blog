---
title: CSS-CSS弹性盒子
date: 2018-04-12 14:04:53
tags: CSS
categories: Web
---

一、弹性盒子使用场景		

浮动floats 和 定位positioning能满足大部分的布局，但也有局限性：

	1.垂直居中父内容的里一块内容。
	2.使容器的所有子项占用等量的可用宽度/高度，而不管有多少宽度/高度可用。
	3.使多列布局中的所有列采用相同的高度，即使它们包含的内容量不同。
二、例子	
![flexbox-example1](flexbox-example1.png)

```html
<body>
    <header>
        <h1>Sample flexbox example</h1>
      </header>
  
      <section>
        <article>
          <h2>First article</h2>
  
          <p>Tacos actually microdosing, pour-over semiotics banjo chicharrones retro fanny pack portland everyday carry vinyl typewriter. Tacos PBR&B pork belly, everyday carry ennui pickled sriracha normcore hashtag polaroid single-origin coffee cold-pressed. PBR&B tattooed trust fund twee, leggings salvia iPhone photo booth health goth gastropub hammock.</p>
        </article>
  
        <article>
          <h2>Second article</h2>
  
          <p>Tacos actually microdosing, pour-over semiotics banjo chicharrones retro fanny pack portland everyday carry vinyl typewriter. Tacos PBR&B pork belly, everyday carry ennui pickled sriracha normcore hashtag polaroid single-origin coffee cold-pressed. PBR&B tattooed trust fund twee, leggings salvia iPhone photo booth health goth gastropub hammock.</p>
        </article>
  
        <article>
          <h2>Third article</h2>
  
          <p>Tacos actually microdosing, pour-over semiotics banjo chicharrones retro fanny pack portland everyday carry vinyl typewriter. Tacos PBR&B pork belly, everyday carry ennui pickled sriracha normcore hashtag polaroid single-origin coffee cold-pressed. PBR&B tattooed trust fund twee, leggings salvia iPhone photo booth health goth gastropub hammock.</p>
  
          <p>Cray food truck brunch, XOXO +1 keffiyeh pickled chambray waistcoat ennui. Organic small batch paleo 8-bit. Intelligentsia umami wayfarers pickled, asymmetrical kombucha letterpress kitsch leggings cold-pressed squid chartreuse put a bird on it. Listicle pickled man bun cornhole heirloom art party.</p>
        </article>
      </section>
</body>
```
```css
html {
  font-family: sans-serif;
}
body {
  margin: 0;
}
header {
  background: purple;
  height: 100px;
}
h1 {
  text-align: center;
  color: white;
  line-height: 100px;
  margin: 0;
}
article {
  padding: 10px;
  margin: 10px;
  background: aqua;
}
/* Add your flexbox CSS below here */

```
1.指定元素布局为flexible		
需要选择将哪些元素将设置为柔性的盒子。给这些 flexible 元素的父元素 display 设置一个特定值。

```css
/* Add your flexbox CSS below here */
section{

  display: flex;//三列
  /* flex-direction: column;  三行*/
}
解析：在本例中，我们想要设置 <article> 元素，因此我们给 <section>（变成了 flex 容器）设置 display。
	多列布局具有大小相等的列，并且列的高度都是一样。 这是因为这样的 flex 项（flex容器的子项）的默认值是可以解决这些的常见问题的.
```
2.flex模型说明		
当元素表现为flex框时，它们沿着两个轴来布局:	
![flex_terms](flex_terms.png)		
	
	主轴（main axis）是沿着 flex 元素放置的方向延伸的轴（比如页面上的横向的行、纵向的列）。该轴的开始和结束被称为 main start 和 main end。
	交叉轴（cross axis）是垂直于 flex 元素放置方向的轴。该轴的开始和结束被称为 cross start 和 cross end。
	设置了 display: flex 的父元素（在本例中是 <section>）被称之为 flex 容器（flex container）。
	在 flex 容器中表现为柔性的盒子的元素被称之为 flex 项（flex item）（本例中是 <article> 元素
3.弹性盒子属性：flex-direction 指定主轴的方向(弹性盒子子类放置的地方)默认row,使得它们按照浏览器的默认语言方向排成一排。

```css
flex-direction: column;
```	
4.换行		
当布局中使用定宽或者定高的时候，可能会出现一个问题：处于容器中的弹性盒子子元素会溢出，破坏布局。		
![flexbox-example3](flexbox-example3.png)     
解决方案:

```css
flex-wrap: wrap;
```
![flexbox-example4](flexbox-example4.png)

```css
/* Add your flexbox CSS below here */
section {
  display: flex;
  flex-direction: row;//默认从左往右开始排列
  flex-direction: row-reverse;//从右往左开始排列
  flex-wrap: wrap;
}
article {
  flex: 200px;//每个元素的宽度至少是200px
}

```	
	多行弹性盒子 - 任何溢出的元素将被移到下一行；
	最后一行上的最后几个项每个都变得更宽，以便把整个行填满;
5.felx-flow缩写		
flex-direction 和 flex-wrap — 的缩写 flex-flow:

```css
flex-direction: row;
flex-wrap: wrap;
等价于
flex-flow: row wrap;
```
6.flex项的动态尺寸			

```css
//无单位的比例值，表示每个flex项沿主轴的可用空间大小
article{
	flex: 1;
}
解析：
	表示每个元素占用空间都是相等的，占用的空间是设置padding和margin之后剩余的空间(因为是个比例flex: 1和flex: 20的效果一样)
```
```css
article {
  flex: 1;
}
article:nth-of-type(3){

  flex: 2;
}
解析：
	第三个 <article> 元素占用了两倍的可用宽度；
```
```css
article {
  flex: 1 200px;
}

article:nth-of-type(3) {
  flex: 2 200px;
}
解析:
	每个flex 项将首先给出200px的可用空间，然后，剩余的可用空间将根据分配的比例共享
```
7.flex:缩写与全写		
flex 是一个可以指定最多三个不同值的缩写属性：

	第一个就是上面所讨论过的无单位比例。可以单独指定全写 flex-grow 属性的值。
	第二个无单位比例 — flex-shrink — 一般用于溢出容器的 flex 项。这指定了从每个 flex 项中取出多少溢出量，以阻止它们溢出它们的容器。 这是一个相当高级的弹性盒子功能，我们不会在本文中进一步说明。
	第三个是上面讨论的最小值。可以单独指定全写 flex-basis 属性的值
tips:建议不要使用全写属性，除非你真的需要（比如要去覆盖之前写的）。使用全写会多些很多的代码，它们也可能有点让人困惑.    
三、水平和垂直对齐	

```html
<div>
    <button>Smile</button>
    <button>Laugh</button>
    <button>Wink</button>
    <button>Shrug</button>
    <button>Blush</button>
</div>
```
```css
html {
	font-family: sans-serif;
}
body {
	width: 70%;
	max-width: 960px;
	margin: 20px auto;
}
button {
	font-size: 18px;
	line-height: 1.5;
	width: 15%;
}
div {
	height: 100px;
	border: 1px solid black;
}
/* Add your flexbox CSS below here */

```
![flex-align1](flex-align1.png)

1.添加css		

```css
div{
	display: flex;
	align-items: center;
	justify-content: space-around;
}
解析:	
	align-items 控制 flex 项在交叉轴上的位置。

		默认的值是 stretch，其会使所有 flex 项沿着交叉轴的方向拉伸以填充父容器。如果父容器在交叉轴方向上没有固定宽度（即高度），则所有 flex 项将变得与最长的 flex 项一样长（即高度保持一致）。
		使用 center 值会使这些项保持其原有的高度，但是会在交叉轴居中。这就是那些按钮垂直居中的原因。
		也可以设置诸如 flex-start 或 flex-end 这样使 flex 项在交叉轴的开始或结束处对齐所有的值.
		可以用align-self属性覆盖align-items属性的行为：
			button:first-child{
				align-self: flex-end;
			}
	justify-content 控制 flex 项在主轴上的位置。

		默认值是 flex-start，使所有 flex 项都位于主轴的开始处。
		也可以用 flex-end 来让 flex 项到结尾处。
		center在justify-content里也是可用的，可以让 flex 项在主轴居中。
		用到的值 space-around 是很有用的——使所有 flex 项沿着主轴均匀地分布，在任意一端都会留有一点空间。
		还有一个值是 space-between，它和 space-around 非常相似，只是它不会在两端留下任何空间。
```
![flex-align2.png](flex-aligm2.png)   
2.flex项排序		
弹性盒子也有可以改变 flex 项的布局位置的功能，而不会影响到源顺序（即 dom 树里元素的顺序）。这也是传统布局方式很难做到的一点。

```css
button:first-child{
	order: 1;
}
解析:"Smile" 按钮移动到了主轴的末尾
	1.所有 flex 项默认的 order 值是 0。
	2.order 值大的 flex 项比 order 值小的在显示顺序中更靠后。
	3.相同 order 值的 flex 项按源顺序显示。所以假如你有四个元素，其 order 值分别是2，1，1和0，那么它们的显示顺序就分别是第四，第二，第三，和第一。
	4.第三个元素显示在第二个后面是因为它们的 order 值一样，且第三个元素在源顺序中排在第二个后面.
	5.也可以给 order 设置负值使它们比值为 0 的元素排得更前面。比如，你可以设置 "Blush" 按钮排在主轴的最前面;
		button:last-child {
		  order: -1;
		}
```
3.flex嵌套		
![flexbox-example7.png](flexbox-example7.png)    

```html
<body>
    <header>
        <h1>Complex flexbox example</h1>
      </header>
  
      <section>
        <article>
          <h2>First article</h2>
  
          <p>Tacos actually microdosing, pour-over semiotics banjo chicharrones retro fanny pack portland everyday carry vinyl typewriter. Tacos PBR&B pork belly, everyday carry ennui pickled sriracha normcore hashtag polaroid single-origin coffee cold-pressed. PBR&B tattooed trust fund twee, leggings salvia iPhone photo booth health goth gastropub hammock.</p>
        </article>
  
        <article>
          <h2>Second article</h2>
  
          <p>Tacos actually microdosing, pour-over semiotics banjo chicharrones retro fanny pack portland everyday carry vinyl typewriter. Tacos PBR&B pork belly, everyday carry ennui pickled sriracha normcore hashtag polaroid single-origin coffee cold-pressed. PBR&B tattooed trust fund twee, leggings salvia iPhone photo booth health goth gastropub hammock.</p>
        </article>
  
        <article>
          <div>
            <button>Smile</button>
            <button>Laugh</button>
            <button>Wink</button>
            <button>Shrug</button>
            <button>Blush</button>
          </div>
          <div>
            <p>Tacos actually microdosing, pour-over semiotics banjo chicharrones retro fanny pack portland everyday carry vinyl typewriter. Tacos PBR&B pork belly, everyday carry ennui pickled sriracha normcore hashtag polaroid single-origin coffee cold-pressed. PBR&B tattooed trust fund twee, leggings salvia iPhone photo booth health goth gastropub hammock.</p>
          </div>
          <div>
            <p>Cray food truck brunch, XOXO +1 keffiyeh pickled chambray waistcoat ennui. Organic small batch paleo 8-bit. Intelligentsia umami wayfarers pickled, asymmetrical kombucha letterpress kitsch leggings cold-pressed squid chartreuse put a bird on it. Listicle pickled man bun cornhole heirloom art party.</p>
          </div>
        </article>
      </section>
</body>
```

```css
html {
  font-family: sans-serif;
}
body {
  margin: 0;
}
header {
  background: purple;
  height: 100px;
}
h1 {
  text-align: center;
  color: white;
  line-height: 100px;
  margin: 0;
}
article {
  padding: 10px;
  margin: 10px;
  background: aqua;
}
/* Add your flexbox CSS below here */
section {
  display: flex;//设置<section>的子代布局为flexible box；
}
//设置一些 flex 值
article {
  flex: 1 200px;
}

//设置第三个 <article> 元素里的子元素同样表现为 flex 项，但是这次我们使它们放置为列
article:nth-of-type(3) {
  flex: 3 200px;
  display: flex;
  flex-flow: column;
}

//选择了第一个 <div>。首先使用 flex: 1 100px; 简单的给它一个最小的高度 100px，
然后设置它的子代（<button> 元素）为 flex 项。在这里我们将它们放在一个包装行中，使它们居中对齐;
article:nth-of-type(3) div:first-child {
  flex: 1 100px;
  display: flex;
  flex-flow: row wrap;
  align-items: center;
  justify-content: space-around;
}

//给按钮设置大小 给它一个值为1的 flex 属性。
如果调整浏览器窗口宽度，会看到按钮将占用尽可能多的空间，尽可能多的坐在同一条线上;
但是当它们不再适合在同一条线上，他们会到下一行去;
button {
  flex: 1;
  margin: 5px;
  font-size: 18px;
  line-height: 1.5;
}
```
结构如下：

```css
section - article
          article
          article - div - button   
                    div   button
                    div   button
                          button
                          button
```
四、跨浏览器兼容性

在真实网站中使用弹性盒子，则需要进行测试，并确保在尽可能多的浏览器中用户体验仍然可以接受。

如果浏览器缺少 CSS 阴影，则该网站可能仍然可用。 但是假如不支持 弹性盒子功能就会完全打破布局，使其不可用。

参考资料:	
1.[弹性盒子](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/CSS_layout/Flexbox)
