---
title: CSS-CSS定位Demo
date: 2018-04-12 09:07:30
tags: CSS
categories: Web
---
 
一、列表消息盒子 - 经典的选项卡消息框		
![tabbed-info-box](tabbed-info-box.png)  

``` html
<body>
  <section class="info-box">
    <ul>
      <li><a href="#" class="active">Tab 1</a></li>
      <li><a href="#">Tab 2</a></li>
      <li><a href="#">Tab 3</a></li>
    </ul>
    <div class="panels">
      <article class="active-panel">
        <h2>The first tab</h2>
  
        <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque turpis nibh, porttitor nec venenatis eu, pulvinar in augue. Vestibulum et orci scelerisque, vulputate tellus quis, lobortis dui. Vivamus varius libero at ipsum mattis efficitur ut nec nisl. Nullam eget tincidunt metus. Donec ultrices, urna maximus consequat aliquet, dui neque eleifend lorem, a auctor libero turpis at sem. Aliquam ut porttitor urna. Nulla facilisi.</p>
      </article>
      <article>
        <h2>The second tab</h2>
  
        <p>This tab hasn't got any Lorem Ipsum in it. But the content isn't very exciting all the same.</p>
      </article>
      <article>
        <h2>The third tab</h2>
  
        <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque turpis nibh, porttitor nec venenatis eu, pulvinar in augue. And now an ordered list: how exciting!</p>
  
        <ol>
          <li>dui neque eleifend lorem, a auctor libero turpis at sem.</li>
          <li>Aliquam ut porttitor urna.</li>
          <li>Nulla facilisi</li>
        </ol>
      </article>
    </div>
  </section>
</body>
```
1.一般设置

```css
html {
	font-family: scans-serif;//设置字体 - 无衬线
}
* {
	box-sizing: border-box;//使用box-sizing模型
}
body {
	margin: 0;//去掉默认外边距
}
```
```css
//对内容设置具体高度宽度，屏幕剧中margin: a auto;在选项卡中如果不固定标签高度，看起来不一致，因此选择固定尺寸；
.info-box{

  width: 450px;
  height: 400px;
  margin: 0 auto;
}
```
2.样式化选项卡

整个例子 .info-box 都位于链的开始——这样页面已经带有其他内容时，可以插入这个特征，不害怕与应用于页面其他部分的样式冲突；

```css
//从无序列表中移除默认的padding-left和margin-top的值
.info-box ul {
  padding-left: 0;
  margin-top: 0;
}
```
```css
//将样式化水平选项卡-列表左浮动确保一行他们合在一起
.info-box li {
  float: left;
  list-style-type: none;//去除项目符号
  width: 150px;//设置每个选项卡宽度150px-参考info-box宽度450px
}

.info-box li a {
  display: inline-block;//保持样式，在一行显示
  text-decoration: none;
  width: 100%;
  line-height: 3;
  background-color: red;
  color: black;
  text-align: center;
}
```
```css
//在链接状态上设置样式
//设置标签focus和hover在获得焦点/鼠标悬浮的时候看起来不同，给用户提供一些可视化反馈
.info-box li a:focus, .info-box li a:hover {
  background-color: #a60000;
  color: white;
}
//当某个选项卡的类（ class ）出现 active 时，我们为其设置一条相同的样式规则。通过使用JavaScript来设置，当一个标签被点击时
.info-box li a.active {
  background-color: #a60000;
  color: white;
}
```
3.样式化面板	

```css
//去样式化.panels<div>容器
.info-box .panels {
  height: 352px;
  position: relative;//设置<div>为定位上下文，相对自身位置定位子元素；
  clear: both;//清除浮动，避免对影响后面布局
}
```
```css
//对包含面板的单独<article>元素设置样式
.info-box article {
  position: absolute;//绝对定位面板

  //使内容都位于<div>容器左上角
  top: 0;
  left: 0;
  height: 352px;
  
  padding: 10px;//设置内边距
  color: white;//字体颜色
  background-color: #a60000;
}
//对带有类（class）为active-panles的面板。设置z-index = 1，使其位于其他面板之上，合适的时候调用JS来添加该类
.info-box .active-panel {
  z-index: 1;
}

```
4.添加JavaScript   	

```js
//定义所有选项卡变量-tabs
var tabs = document.querySelectorAll('.info-box li a');
//定义所有面板变量-panles
var panels = document.querySelectorAll('.info-box article');

for(i = 0; i < tabs.length; i++) {
  var tab = tabs[i];
  setTabHandler(tab, i);
}

//当每个选项卡被点击时应该发生的功能，运行时，函数会被传递选项卡和一个索引数i指明选项卡在tabs数组中的位置。
function setTabHandler(tab, tabPos) {
	
	//创建点击事件
  tab.onclick = function() {
  	//点击事件处理
    for(i = 0; i < tabs.length; i++) {
    	
    	//如果选项卡tab的链接<a>的class属性存在，则移除
      if(tabs[i].getAttribute('class')) {
      
        tabs[i].removeAttribute('class');
      }
    }
	
	//设置当前点击的选项卡tab 的class属性
    tab.setAttribute('class', 'active');

	//配置面板信息
    for(i = 0; i < panels.length; i++) {
    
      if(panels[i].getAttribute('class')) {
      
        panels[i].removeAttribute('class');
      }
    }
	//在css中面板active-panel的会这是z-index=1； 显示在最上层
    panels[tabPos].setAttribute('class', 'active-panel');
  }
}
```
二、固定位置的列表消息盒子		
![fixed-info-box](fixed-info-box.png)  

在上述例子的基础上 - 将固定消息盒子的位置，以便于他能待在浏览器窗口的同一个位置。当主要内容滚动时，这个消息盒子将会待在屏幕的同一个位置。     
1.添加HTML

```html
在上述例子的html文件的</section>标签处添加
<section class="fake-content">
      <h1>Fake content</h1>
      <p>This is fake content. Your main web page contents would probably go here.</p>
      <p>This is fake content. Your main web page contents would probably go here.</p>
      <p>This is fake content. Your main web page contents would probably go here.</p>
      <p>This is fake content. Your main web page contents would probably go here.</p>
      <p>This is fake content. Your main web page contents would probably go here.</p>
      <p>This is fake content. Your main web page contents would probably go here.</p>
      <p>This is fake content. Your main web page contents would probably go here.</p>
      <p>This is fake content. Your main web page contents would probably go here.</p>
  </section>

```
2.更改CSS

```css
修改上述例子中css文件的.info-box内容如下:
//删除你的margin: 0 auto （不需要居中显示），添加fixed定位；调整top 属性把她粘在浏览器的视域

.info-box{

  /* width: 450px;
  height: 400px;
  margin: 0 auto; */
  width: 450px;
  height: 400px;
  position: fixed;
  top: 0;
}
```
3.设计主内容样式

```css
//添加 背景颜色，字体颜色，内边距，设置margin-left使他移动到右边，为消息盒子在左边腾出位置，以便于各个部分不重叠。
.fake-content {
  background-color: #a60000;
  color: white;
  padding: 10px;
  height: 2000px;
  margin-left: 470px;
}
```
三、滑动隐藏的面板		
通过按图标使面板滑动出现或者消失，这种场景多用于移动端布局。    
![hidden-sliding-panel](hidden-sliding-panel.png)		

```html
<body>
	/**
	 * label的for 属性规定label属性绑定到哪个表单元素
	 * 一个label 元素和input元素——<label>元素普遍用来联系文字标签和表单，目的是能更好的理解表单（允许用户查看表单元素的描述）。这里通过for属性绑定id到了<input>标签的checkbox元素.*/
	
    <label for="toggle">❔</label>
    <input type="checkbox" id="toggle">
    <aside>
      <h2>Information</h2>
  
      <p>Some very important information about your app:</p>
  
      <ol>
        <li>It has a really cool slide-out information box.</li>
        <li>This information box uses a combination of fixed positioning and a CSS transition for the smooth sliding.</li>
        <li>It also uses a cool technique called the <a href="https://css-tricks.com/the-checkbox-hack/">checkbox hack</a>.</li>
        <li>This allows you to create a nice "toggle on/toggle off" UI effect without using any JavaScript, which will work in IE9 and above (the smooth transition will work in IE10 and above.)</li>
  
      </ol>
    </aside>
  <script src="css-demo3.js"></script>
</body>
```
1.设置表单元素样式

```css
 /* || Checkbox hack to control information box display */
 label[for="toggle"] {//设置label样式
  font-size: 3rem;
  position: absolute;
  top: 4px;
  right: 5px;
  z-index: 1;
  cursor: pointer;//使用cursor属性来说改变鼠标的指针，当鼠标悬浮在图标上面的时候变成一个手形指针（就像你看到的当悬浮在链接上一样），作为一个额外的可视化线索告诉用户这个图标可以做一些有趣的事情。
}

//设置绝对定位属性，并隐藏在顶部的上面，不希望在用户界面显示
input[type="checkbox"] {
   position: absolute;
   top: -100px;
}
```
2.设置面板样式

```css
/* information box styling */
aside {
  background-color: #a60000;
  color: white;
  width: 340px;
  height: 100%;
  padding: 0 20px;
  
  //设置面板的定位为fixed，即使页面的内容在滚动，也总是显示在同一个位置。我们把设置top属性让窗口粘在顶部，然后设置默认情况下远离屏幕，设置right属性使其位于屏幕的右边；
  position: fixed;
  top: 0;
  right: -370px;

  //允许你在状态改变的时候平滑的过渡，而不是粗暴的“变”或“还原”。
  transition: 0.6s all;
}
```
4.设置选择后的状态

```css
/* Second part of the checkbox hack — the checked state */
input[type=checkbox]:checked + aside {

//将right属性设置为0px，会造成面板再次出现在屏幕上（由于过渡属性会平滑的出现）。再一次点击这个标签会取消选中checkbox，面板将会跟着再一次消失
  right: 0px;
}
```

参考资料：	
1.[定位实例练习](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/CSS_layout/Practical_positioning_examples)
