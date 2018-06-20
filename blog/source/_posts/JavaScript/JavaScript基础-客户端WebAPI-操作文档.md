---
title: JavaScript基础-客户端WebAPI-操作文档
date: 2018-04-23 11:30:08
tags: JavaScript
categories: Web
---

一、Web浏览器的组成		

<!--more-->
	
![document-window-navigator](document-window-navigator.png)		
	
1.window

	载入浏览器的标签，在JavaScript中用Window对象来表示，
	使用这个对象的可用方法，可以返回窗口的大小（参见Window.innerWidth和Window.innerHeight），操作载入窗口的文档，存储客户端上文档的特殊数据（例如使用本地数据库或其他存储设备），为当前窗口绑定event handler，等等
2.navigator

	表示浏览器存在于web上的状态和标识（即用户代理）。在JavaScript中，用Navigator来表示。
	可以用这个对象获取一些信息，比如来自用户摄像头的地理信息、用户偏爱的语言、多媒体流等等
3.document

	（在浏览器中用DOM表示）是载入窗口的实际页面，在JavaScript中用Document 对象表示，你可以用这个对象来返回和操作文档中HTML和CSS上的信息。
	例如获取DOM中一个元素的引用，修改其文本内容，并应用新的样式，创建新的元素并添加为当前元素的子元素，甚至把他们一起删除
二、DOM		
1.DOM(文档对象模型)：在浏览器标签中当前载入的文档. 浏览器生成的‘树结构’，可以用JavaScript操作DOM.     
2.树结构			

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Simple DOM example</title>
  </head>
  <body>
      <section>
        <img src="dinosaur.png" alt="A red Tyrannosaurus Rex: A two legged dinosaur standing upright like a human, with small arms, and a large head with lots of sharp teeth.">
        <p>Here we will add a link to the <a href="https://www.mozilla.org/">Mozilla homepage</a></p>
      </section>
  </body>
</html>
```
![dom-screenshot](dom-screenshot.png)	

	节点：文档中每个元素和文本在树中都有它们自己的入口;
	元素节点：一个元素，存在于DOM中;
	根结点：树中顶层节点，在HTML的情况下，总是一个HTML节点(其他标记词汇，如SVG和定制XML将有不同的根元素);
	子节点:直接位于另一个节点内的节点；
	后代子节点:位于另一个节点内任意位置的节点；
	父节点：里面有另一个节点的节点；（section）
	兄弟节点：DOM树中位于同一等级的节点;(上例子中的IMG和P)
	文本节点：包含文字串的节点；
三、DOM基本操作		
步骤:		
1.添加HTML实例,在&lt;/body&gt;上一行加入&lt;script&gt;&lt;/script&gt;元素；

```html
<body>
  <section>
    <img src="dinosaur.png" alt="A red Tyrannosaurus Rex: A two legged dinosaur standing upright like a human, with small arms, and a large head with lots of sharp teeth.">
    <p>Here we will add a link to the <a href="https://www.mozilla.org/">Mozilla homepage</a></p>
  </section>  
  <script src="js-demo.js"></script>
</body> 
```
2.JavaScript中操作DOM元素		

```js
//可以使用link的可以属性和方法操作它
//允许使用CSS选择器选择元素，推荐主流方法
var link = document.querySelector('a');
//修改属性值
link.textContent = '这是操作DOM修改的内容';
//修改链接
link.href = 'https://www.baidu.com';
```
![edit-dom](edit-dom.png)		
解析：	
	
	Document.querySelector()匹配它在文档中遇到的第一个<a>元素；
	Document.querySelectorAll()匹配文档中每个匹配选择器的元素，并把它们的引用存储在一个array中；
	获取元素方法:
		Document.getElementById()选择一个id属性值已知的元素；
		Document.getElementsByTagName()返回页面中包含的所有已知类型元素的数组；
3.创建并放置新的节点		

```js
//获取添加位置
var sect = document.querySelector('section');
//创建新的段落，赋值文本
var para = document.createElement('p');
para.textContent = '这是一个创建的对象';
//添加段落元素
sect.appendChild(para);

//创建文本节点
var text = document.createTextNode(' — the premier source for web development knowledge.');
var linkPara = document.querySelector('p');
//绑定节点
linkPara.appendChild(text);
```
4.移动和删除元素		
移动

```js
var sect = document.querySelector('section');
var linkPara = document.querySelector('p');
//绑定节点
linkPara.appendChild(text);
//移动元素位置(从section节点移到p节点)
sect.appendChild(text)
```
删除		
拥有要删除的节点和其父节点的引用。在当前情况下，我们只要使用Node.removeChild()即可		

```js
sect.removeChild(linkPara);
要删除一个仅基于自身引用的节点可能稍微有点复杂，这也是很常见的。
删除自己。
linkPara.parentNode.removeChild(linkPara);
```
5.操作样式			
方法一：内联样式 - 动态设置样式的内部添加内联样式

```js	
用HTMLElement.style属性来实现:
	
para.style.color = 'red';
para.style.backgroundColor = 'black';
para.style.padding = '10px';
para.style.width = '250px';
para.style.textAlign = 'center';	

内联样式：控制台查看
<p style="color: red; background-color: black; padding: 10px; width: 250px; text-align: center;">这是一个创建的对象</p>
```
![edit-style](edit-style.png)		
方法二：非内联样式 - 在HTML添加代码到&lt;head&gt;中		

```html
<head>
	<style>
		.highlight{
			color: red;
			background-color: black;
			padding: 10px;
			width: 250px;
			text-align: center;
		}
	</style>
</head>
```
```js
para.setAttribute('class','highlight');
```

方法一无需安装，适合简单应用；		
方法二更佳传统(没有CSS和JavaScript的混用)	
四、从Window对象获取信息		
动态改变视窗大小		

```html
<head>
  <meta charset="UTF-8">
  <link rel="stylesheet" href="js-demo.css">
  <style>
    body {
        margin: 0;
      }
      div {
        box-sizing: border-box;
        width: 100px;
        height: 100px;
        background-image: url(bgtile.png);
        border: 10px solid white;
      }
	</style>
</head>
<body>

    <div>

    </div>
  <script src="js-demo.js"></script> 
</body>
```
```js
//3.获取Windows信息
var WIDTH = window.innerWidth;//屏幕宽度
var HEIGHT = window.innerHeight;//屏幕高度

var div = document.querySelector('div');

div.style.width = WIDTH + 'px';
div.style.height = HEIGHT + 'px';

//每次窗口调整大小时都会触发该事件
window.onresize = function(){

    WIDTH = window.innerWidth;
    HEIGHT = window.innerHeight;
    div.style.width = WIDTH + 'px';
    div.style.height = HEIGHT + 'px';
}
```
五、一个动态的购物单		
![shopping-list](shopping-list.png)		

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>购物单</title>
  <link rel="stylesheet" href="js-demo.css">
</head>
<body>
  
  <h1>My shopping list</h1>
  <div>
    <label for="item">Enter a new item:</label>
    <input type="text" name="item" id='item'>
    <button>Add item</button>
  </div>
  <ul>
    
  </ul>
  <script src="js-demo.js"></script>
</body>
</html>
```
```css
li {
  margin-bottom: 10px;
}
li button {
  font-size: 8px;
  margin-left: 20px;
  color: #666;
}
```
```js
//创建三个变量
var button = document.querySelector('button');
var list = document.querySelector('ul');
var input = document.querySelector('input');

//创建按钮事件
button.onclick = function(){
	
	//接收输入框的内容并置空输入框
    var currentInput = input.value;
    input.value = ' ';

    var deleteButton = document.createElement('button');
    var listSpan = document.createElement('span');
    var listItem = document.createElement('li');

    listItem.appendChild(listSpan);
    listSpan.textContent = currentInput;

    listItem.appendChild(deleteButton);
    deleteButton.textContent = 'Delete';

    list.appendChild(listItem);
    
    //创建删除按钮事件函数
    deleteButton.onclick = function(){
	
		//删除listItem元素
        list.removeChild(listItem);
    }
    //输入框光标响应
    input.focus();
};
```

参考资料：	
1.[客户端网页API](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Client-side_web_APIs)  
2.[操作文档](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Client-side_web_APIs/Manipulating_documents)		
3.[Node API](https://developer.mozilla.org/zh-CN/docs/Web/API/Node) 		
4.[Document](https://developer.mozilla.org/zh-CN/docs/Web/API/Document)		
 