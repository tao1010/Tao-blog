---
title: JavaScript基础-JS代码块
date: 2018-04-17 09:03:54
tags: JavaScript
categories: Web
---

一、条件控制语句

<!--more-->
		
1.if...else		
2.else if		
3.关于比较运算符
	
	=== and !== — 判断一个值是否严格等于，或不等于另一个。
	< and > — 判断一个值是否小于，或大于另一个。
	<= and >= — 判断一个值是否小于或等于，或者大于或等于另一个。
任何不是 false, undefined, null, 0, NaN 的值，或一个空字符串（''）在作为条件语句进行测试时实际返回true，因此您可以简单地使用变量名称来测试它是否为真，甚至是否存在（即它不是未定义的）
	
	var cheese = 'Cheddar';

	if (cheese) {
	  console.log('Yay! Cheese available for making cheese on toast.');
	} else {
	  console.log('No cheese on toast for you today.');
	}
4.嵌套if...else		
	
	if(...){
		if(...){
			...
		}else{
			...
		}
	}else{
		...
	}
5.逻辑运算符：and，or，not
	
	&& — 逻辑与; 使得并列两个或者更多的表达式成为可能，只有当这些表达式每一个都返回true时，整个表达式才会返回true；
	|| — 逻辑或; 当两个或者更多表达式当中的任何一个返回 true 则整个表达式将会返回 true；
	NOT, 用 ! 运算符表示, 可以用于对一个表达式取否；
6.switch
	
	switch(表达式/值){
		case choice1:
		{...}
		break;
		case choice2:
		{...}
		break;
		default:
		{...}
	}
	choice:表达式/值
7.三元运算符
	
	表达式1 ？ 表达式2 : 表达式3;	
二、循环语句	
	
	for
	while
	do...while
	
1.使用break - 退出循环	
2.使用continue - 跳过迭代,进入下一次循环		
3.while和do...while	
必须保证初始变量是迭代的，那么它才会逐渐地达到退出条件；
三、函数模块
1.函数与方法
	
	内置浏览器函数并不是函数，它是方法；
	方法是在对象内定义的函数，内置浏览器功能（方法）和变量（称为属性）存储在结构化对象内，以使代码更加高效，易于处理；
2.自定义函数		
function 函数名称(){...}		
定义一个函数的关键字 function开始，函数名；一组括号；和一组大括号。函数的任何参数都在括号内，当我们调用该函数时运行的代码均在大括号内；

```js
//使用了一个DOM（文档对象模型）的内置方法 document.querySelector() 来选择<html> 元素并且把它存放在一个叫 html的变量中,方便后续使用
var html = document.querySelector('html');

//创建 <div> 元素并且把该新建元素的引用（实际上是新建对象的地址）放在一个叫做 panel的变量中。 这个元素将成为我们的消息框的外部容器。
var panel = document.createElement('div');
//给panel元素添加了一个值为msgBox 的class 类属性
panel.setAttribute('class', 'msgBox');
//给 html变量追加设置好样式的panel元素 。该方法追加了元素的同时也把panel<div>元素指定为<html>的子元素；
html.appendChild(panel);

var msg = document.createElement('p');
msg.textContent = 'This is a message box';
panel.appendChild(msg);

var closeBtn = document.createElement('button');
closeBtn.textContent = 'x';
panel.appendChild(closeBtn);

//使用一个叫做 GlobalEventHandlers.onclick 的事件句柄给按钮添加了一个点击事件, 点击事件后定义了一个匿名函数，功能是将消息提示框从父容器中删除 — 达到了关闭的效果。
closeBtn.onclick = function() {
	
//panel.parentNode就是指panel的上一级，就是整个DOM，然后再来用这个父亲来干掉这个儿子，儿子不能自己干掉自己
  panel.parentNode.removeChild(panel);
}
```
3.函数调用
	
	//定义
	function myFunction(){
		...
	}
	//调用
	myFunction();
4.匿名函数	
通常使用匿名函数以及事件处理程序：

	var myButton = document.querySelector('button');
	myButton.onclick = function() {
	  alert('hello');
	}
匿名函数也称为函数表达式。函数表达式与函数声明有一些区别。函数声明会进行声明提升（declaration hoisting），而函数表达式不会.    
5.函数参数		
浏览器的内置Math.random（）函数不需要任何参数。当被调用时，它总是返回0到1之间的随机数：
	
	var myNumber = Math.random();
浏览器的内置字符串replace（）函数需要两个参数：
	
	var myText = 'I am a string';
	var newString = myText.replace('string', 'sausage');

	当您需要指定多个参数时，它们以逗号分隔;
6.函数作用域和冲突		
所有函数的最外层被称为全局作用域。 在全局作用域内定义的值可以在任意地方访问；		
有一个HTML文件，它调用两个外部JavaScript文件，并且它们都有一个使用相同名称定义的变量和函数：

```html
<!-- Excerpt from my HTML -->
<script src="first.js"></script>
<script src="second.js"></script>
<script>
  greeting();
</script>
```
```js
// first.js
var name = 'Chris';
function greeting() {
  alert('Hello ' + name + ': welcome to our company.');
}
```
```js
// second.js
var name = 'Zaptec';
function greeting() {
  alert('Our company is called ' + name + '.');
}
```
解析:
	
	这两个函数都使用 greeting() 形式调用，但是你只能访问到 second.js文件的greeting()函数。second.js 在源代码中后应用到HTML中，所以它的变量和函数覆盖了 first.js 中的.

相同的范围规则不适用于循环（for（）{...}）和条件块（if（）{...}）;		
ReferenceError：“x”未定义错误是您遇到的最常见的错误。如果您收到此错误，并且确定您已经定义了该问题的变量，请检查它的范围;    
7.函数内部函数		

	function myBigFunction() {
	  var myValue = 1;
	      
	  subFunction1(myValue);
	  subFunction2(myValue);
	  subFunction3(myValue);
	}
	
	function subFunction1(value) {
	  console.log(value);
	}
	
	function subFunction2(value) {
	  console.log(value);
	}
	
	function subFunction3(value) {
	  console.log(value);
	}
8.函数返回值	
	
	var myText = 'I am a string';
	var newString = myText.replace('string', 'sausage');
	console.log(newString);
自定义函数中使用返回值:
	
	function random(number) {
		 return Math.floor(Math.random()*number);
	}
四、事件
1.概念:事件是在编程时系统内的发生的动作或者发生的事情，系统通过它来告诉您在您愿意的情况下您可以以某种方式对它做出回应.    
2.每个可用的事件都会有一个事件处理器(事件触发时会运行的代码块)。

注册事件处理器:当定义了一个用来回应事件被激发的代码块的时候.

事件处理器有时候被叫做事件监听器——从我们的用意来看这两个名字是相同的，尽管严格地说来这块代码既监听也处理事件。监听器留意事件是否发生，然后处理器就是对事件发生做出的回应。

<font color='red'>tips： 网络事件不是 JavaScript 语言的核心——它们被定义成内置于浏览器的JavaScript APIs。</font>     
3.使用网页事件的方式		

```html
HTML
<button>Change color</button>
```
```js
JS
方式一：事件处理器属性
var btn = document.querySelector('button');

function random(number) {
  return Math.floor(Math.random()*(number+1));
}

btn.onclick = function() {
  var rndCol = 'rgb(' + random(255) + ',' + random(255) + ',' + random(255) + ')';
  document.body.style.backgroundColor = rndCol;
}
方式二：
var btn = document.querySelector('button');

function bgChange() {
  var rndCol = 'rgb(' + random(255) + ',' + random(255) + ',' + random(255) + ')';
  document.body.style.backgroundColor = rndCol;
}

btn.onclick = bgChange;
方式三：行内事件处理器 - 勿用
HTML：
<button onclick="bgChange()">Press me</button>
JS：
function bgChange() {
  var rndCol = 'rgb(' + random(255) + ',' + random(255) + ',' + random(255) + ')';
  document.body.style.backgroundColor = rndCol;
}
```
4.addEventListener() 和 removeEventListener()

	var btn = document.querySelector('button');
	
	function bgChange() {
	  var rndCol = 'rgb(' + random(255) + ',' + random(255) + ',' + random(255) + ')';
	  document.body.style.backgroundColor = rndCol;
	}   
	
	btn.addEventListener('click', bgChange);
	btn.parentNode.removeEventListener(btn);
	
5.其他事件概念		
事件对象
	
	有时候在事件处理函数内部，您可能会看到一个固定指定名称的参数，例如event，evt或简单的e。它被自动传递给事件处理函数，以提供额外的功能和信息.
阻止默认行为	
	
	在事件对象上调用preventDefault()函数，这样就停止.
	var form = document.querySelector('form');
	var fname = document.getElementById('fname');
	var lname = document.getElementById('lname');
	var submit = document.getElementById('submit');
	var para = document.querySelector('p');
	
	form.onsubmit = function(e) {
	  if (fname.value === '' || lname.value === '') {
	    e.preventDefault();
	    para.textContent = 'You need to fill in both names!';
	  }
	}
事件冒泡及捕获	
当一个事件发生在具有父元素的元素上时，现代浏览器运行两个不同的阶段 - 捕获阶段和冒泡阶段。

```js 
在捕获阶段：

浏览器检查元素的最外层祖先<html>，是否在捕获阶段中注册了一个onclick事件处理程序，如果是，则运行它。
然后，它移动到<html>中的下一个元素，并执行相同的操作，然后是下一个元素，依此类推，直到到达实际点击的元素。

在冒泡阶段，恰恰相反:

浏览器检查实际点击的元素是否在冒泡阶段中注册了一个onclick事件处理程序，如果是，则运行它
然后它移动到下一个直接的祖先元素，并做同样的事情，然后是下一个，等等，直到它到达<html>元素。
```
在现代浏览器中，默认情况下，所有事件处理程序都在冒泡阶段进行注册.		
事件委托		

五、例子-相片走廊

```html
HTML:
<head>
  <meta charset="UTF-8">
  <title>JS相册长廊</title>
  <link rel="stylesheet" href="js-demo.css">
</head>
<body>
  
  <h1>Image gallery example</h1>

<div class="full-img">
  <img class="displayed-img" src="images/pic1.jpg">
  <div class="overlay"></div>
  <button class="dark">Darken</button>
</div>

<div class="thumb-bar">

</div>
  <script src="js-demo.js"></script>
</body>
```
```css
CSS:
h1 {
  font-family: helvetica, arial, sans-serif;
  text-align: center;
}

body {
  width: 640px;
  margin: 0 auto;
}

.full-img {
  position: relative;
  display: block;
  width: 640px;
  height: 480px;
}

.overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 640px;
  height: 480px;
  background-color: rgba(0,0,0,0);
}

button {
  border: 0;
  background: rgba(150,150,150,0.6);
  text-shadow: 1px 1px 1px white;
  border: 1px solid #999;
  position: absolute;
  cursor: pointer;
  top: 2px;
  left: 2px;
}

.thumb-bar img {
  display: block;
  width: 20%;
  float: left;
  cursor: pointer;
}
```
```js
JS:
var displayedImage = document.querySelector('.displayed-img');
var thumbBar = document.querySelector('.thumb-bar');

var btn = document.querySelector('button');
var overlay = document.querySelector('.overlay');

/* Looping through images */
for(i = 0; i < 5;i ++){

  var newImage = document.createElement('img');
  var imagePath = 'images/pic' + (i+1) + '.jpg';
  newImage.setAttribute('src', imagePath);
  thumbBar.appendChild(newImage);
  newImage.onclick = function(index){
    
    var imageSrc = index.target.getAttribute('src');
    setDisplayedImage(imageSrc);
  }
}
function setDisplayedImage(imagePath){

  displayedImage.setAttribute('src',imagePath);
}

/* Wiring up the Darken/Lighten button */
// btn.addEventListener('click',clickButton);
// function clickButton(){
btn.onclick = function(){

  if(btn.getAttribute('class') === 'dark'){

    btn.setAttribute('class', 'light');
    btn.textContent = 'Lighten';
    overlay.style.backgroundColor = 'rgba(0,0,0,0.5)';
  }else{

    btn.setAttribute('class', 'dark');
    btn.textContent = 'Darken';
    overlay.style.backgroundColor = 'rgba(0,0,0,0)';
  }
}
```
解析：

	字符串:overlay.style.backgroundColor = 'rgba(0,0,0,0)';
	set和get方法：
		newImage.setAttribute('src', imagePath);
		var imageSrc = index.target.getAttribute('src');
	for循环的变量：
		for(i = 0; i < 5;i ++){

		  var newImage = document.createElement('img');
		  thumbBar.appendChild(newImage);
		  newImage.onclick = function(index){
		    
		    var imageSrc = index.target.getAttribute('src');
		    
		  }
	}
	
![demo](demo.png)

参考资料：	
1.[代码块](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Building_blocks)		
2.[条件语句](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Building_blocks/conditionals)    
3.[循环语句](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Building_blocks/Looping_code)    
4.[函数](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Building_blocks/Functions)       
5.[自定义函数](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Building_blocks/Build_your_own_function)     
6.[函数返回值](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Building_blocks/Return_values)     
7.[事件](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Building_blocks/Events)     
8.[例子-相片走廊](https://developer.mozilla.org/zh-CN/docs/learn/JavaScript/Building_blocks/相片走廊)