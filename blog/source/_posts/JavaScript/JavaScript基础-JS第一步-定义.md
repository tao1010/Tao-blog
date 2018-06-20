---
title: JavaScript基础-JS第一步-定义
date: 2018-04-11 13:40:46
tags: JavaScript
categories: Web
---

一、JavaScript的定义

<!--more-->

1.JavaScript 是允许你在网页中实现复杂事情的一门编程语言 —— 当你浏览网页时不只是显示静态信息—— 显示即时更新的内容， 或者交互式的地图，或 2D/3D 图形动画，又或者自动播放视频等；

	HTML是一种标记语言，用来结构化我们的网页内容和赋予内容含义，例如定义段落、标题、和数据表,或在页面中嵌入图片和视频。
	CSS 是一种样式规则语言，我们将样式应用于我们的 HTML 内容， 例如设置背景颜色和字体，在多个列种布局我们的内容。
	JavaScript 是一种编程语言，允许你创建动态更新的内容，控制多媒体，图像动画，和一些其他的东西。能够用几行JavaScript代码就能实现。

``` html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <link rel="stylesheet" href="js-demo.css">
</head>
<body>
  <p>Player 1: Chris</p>
</body>

<script src="js-demo.js"></script>
</html>
```
```css
p{

  font-family: 'helvetica neue', Helvetica, sans-serif;
  letter-spacing: 1px;
  text-transform: uppercase;
  text-align: center;
  border: 2px solid rgba(0, 0, 200, 0.6);
  background: rgba(0, 0, 200, 0.3);
  color: rgba(0, 0, 200, 0.6);
  box-shadow: 1px 1px 2px rgba(0, 0, 200, 0.4);
  border-radius: 10px;
  padding: 3px 10px;
  display: inline-block; 
  cursor: pointer;
}
```
```js
var para = document.querySelector('p');
para.addEventListener('click',updateName);
function updateName(){

    var name = prompt('Enter a new name');
    para.textContent = 'Player 1:' + name;
}
```
![js-click](js-click.png)		
二、JS可以做什么?	
解析实例:

	在变量中储存有用的值。请求输入一个新的名字，然后把那个名字储存到一个叫 name 的变量.
	对一段文本（在编程中被称为“字符串”）进行操作。取出字符串 "Player 1: "，然后把它和 name 变量连结起来，创造出完整的文本标签，例：''Player 1: Chris"。
	运行代码以响应在网页中发生的特定事件。用了一个 click 事件来检测按钮什么时候被点击，然后运行更新文本标签的代码;

1.应用程序编程接口APIs：Application Programming Interfaces	
2.浏览器APIs（browser APIs） - 已经安装在浏览器中，能够从周围的计算机环境揭露数据，做其他事

	文档对象模型API[DOM(Document Object Model) API]允许你操作 HTML 和 CSS，创建，移除和修改 HTML，动态地应用新的样式到你的页面，等等。比如说每次你在一个页面里看到一个弹出窗口，或者显示一些新的内容，这就是 DOM 在运作;
	地理位置API [Geolocation API] 获取地理信息。这就是为什么谷歌地图 [Google Maps] 可以找到你的位置，而且标示在地图上;
	画布[Canvas]和WebGL APIs允许创建生动的2D和3D图像；
	音像和影像APIs[Audio and Video APIs] 像 HTMLMediaElement 和 WebRTC 允许你运用多媒体去做一些非常有趣的事情，比如在网页中播放音像和影像，或者从你的网页摄像头中获取获取录像，然后在其他人的电脑上展示;		
3.第三方APIs（Third party APIs）默认是没有安装到浏览器中的，而你通常需要从网络上的某些地方取得它们的代码和信息：

	推特API[Weitter API]允许你去嵌入定制的地图到你的网站，和其他的功能。
	谷歌地图API（Google Maps API）允许你去嵌入定制的地图到你的网站，和其他的功能。
三、JS在页面上做什么？		
	
	在 HTML 和 CSS 已经被集合和组装成一个网页后，浏览器的 JavaScript 引擎执行 JavaScript。这保证了当 JavaScript 开始运行时，网页的结构和样式已经在该出现的地方了；	
 	JavaScript 的普遍用处是通过 DOM API（如之前提及的那样）动态地修改 HTML 和 CSS 来更新用户交界面。
 	如果 JavaScript 在 HTML 和 CSS 加载完成之前加载运行，那么会发生错误。

1.浏览器安全

每个浏览器标签本身就是一个用来运行代码的分离的容器（这些容器用专业术语称为“运行环境”）——这意味着在大多数情况中，每个标签中的代码是完全分离地运行，而且在一个标签中的代码不能直接影响在另一个标签中的代码——或者在另一个网站中的。

2.JS运行顺序 	从上往下执行	
3.解释代码VS编译代码	

	解释语言：JavaScript 是一个解释语言——代码从上到下运行，而运行的结果会马上被返回。在浏览器运行代码前，不必先把它转化为其他形式；
	编译语言则需要在运行前转化为另一种形式。比如说 C/C++ 则要先被编译成汇编语言，然后再由电脑运行；

4.服务器代码VS客户端代码		
	
	客户端代码：在用户的电脑上运行的代码——当浏览一个网页时，这个网页的客户端代码就会被下载，然后由浏览器来运行和展示。在这个 JavaScript 模块，我们将会明确地探讨 客户端 JavaScript [client-side JavaScript]；
	服务器端代码：在服务器上运行，然后它的结果会由浏览器进行下载和展示。流行的服务器端网页语言包含以下几个例子：PHP, Python, Ruby, ASP.NET 和 JavaScript！JavaScript 同时也能用作服务器端语言，比如说在流行的 Node.js；
	
	动态：用来描述客户端 JavaScript 和服务器端语言——它指的是能更新网页/应用的内容以在不同环境下显示不同事物，当有需要时产生新内容的能力。服务器端代码会动态地在服务器上产生新的内容，比如说从数据库中提取信息。反之，客户端 JavaScript则在用户的浏览器中动态地生成新的内容，比如说创建一个新的 HTML 表格，从中插入从服务器请求到的数据，然后在已经向用户展示了的网页中显示这个表格。
	静态：一个没有动态更新内容的网页，只会一直显示一样的内容；

四、页面添加JS的方法		
1.内部的JavaScript		
	
	1>创建xx.html文件
	2>在xx.html文件中的</body>标签前添加如下代码：
		<script>
			//JavaScript goes here
			...
		</script>
	3>在<script>...</script>中完成功能代码
	4>保存html文件，刷新网页
2.外部的JavaScript		
	
	1>创建xx.html xx.js文件
	2>在xx.html文件中的</body>标签前添加如下代码：
		<script src="xx.js"></script>
	3>在xx.js文件中实现功能代码
	4>保存xx.html xx.js文件，刷新网页
3.内联JavaScript处理器
	
在HTML文件中:

```html
<body>
  <p>Player 1: Chris</p>
  <button onclick="createParagraph()" >click me</button>
  <script>

    function createParagraph() {
      var para = document.createElement('p');
      para.textContent = 'You clicked the button!';
      document.body.appendChild(para);
    }
  </script>

  <!-- <script src="js-demo.js"></script> -->
</body>
```
五、注释

1.单行注视
	
	//	I am a comment
2.多行注视
	
	/*
		I am also 
		a comment
	*/

参考资料：	
1.[什么是JavaScript](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/First_steps/What_is_JavaScript)		
2.[DOM API](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model)		
3.[Geolocation API](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation)		
4.[Canvas API](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API)		
5.[WebGL API](https://developer.mozilla.org/en-US/docs/Web/API/WebGL_API)		
6.[Audio and Video API](https://developer.mozilla.org/en-US/Apps/Fundamentals/Audio_and_video_delivery)		