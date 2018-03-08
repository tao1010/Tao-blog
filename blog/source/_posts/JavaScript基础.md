---
title: JavaScript基础
date: 2018-03-07 09:19:02
tags: JavaScript
---


一、JavaScript简介

JavaScript是一门成熟的动态编程语言，当应用于HTML文档时，可以在网站上提供动态交互性。

JavaScript是非常通用的，简单的可以做轮播、相册、浮动布局、按钮点击事件；复杂的可以创建游戏、2d、3d动画、数据库驱动的综合应用程序...

JavaScript工具功能：
	
	1.应用编程接口（APIs） 内置于浏览器中提供像动态编写的CSS和HTML，抓取操作用户摄像头的数据流盒操作3D图像和音频样品；
	2.第三方APIs允许开发者其他公司网站（facebook等）与自己网站合并功能；
	3.可以将第三方框架和库应用于你的HTML，以快速构建网站和应用。
	
二、JavaScript引用步骤

1.创建 ".js"文件

	eg：在项目中创建一个名为scripts的文件夹，并在文件夹中创建一个main.js的文件
	
2.将.js文件引入html文件中

	eg:打开index.html文件 在</body>的闭合标签之前输入下列代码：
	 <script src = "script/main.js"></script>
	
	将<script>元素放在HTML文件底部的原因是，浏览器按照代码在文件中的顺序解析 HTML。
	如果JavaScript在最前面被加载，HTML还未加载，JavaScript将无法作用于HTML，所以JavaScript无效，
	如果 JavaScript 代码出现问题则 HTML 不会被加载。
	所以将 JavaScript 代码放在底部是最好的选择。
	
3.编辑main.js文件
	
	eg：var myHeading = document.querySelector('h1');
		 myHeading.innerHTML = 'Hello world!';
		 
4.保存相关文件（Html、JavaScript）,浏览器打开index.html
	
三、JavaScript基础速览

1.变量（Variables） var

	行末的分号表示语句结束，不过这个分号只有在单行内需要分割语句时才是必须的。然而，一些人认为在每个语句后面加分号是一种好的风格。

	JavaScript 是对大小写敏感的——myVariable 与  myvariable 是不同的。如果你的代码出现问题了，查看一下大小写有没有错误！

2.数据类型

![变量的数据类型](js数据类型.png)

3.注释
	
	多行 /* 000000 */
	单行 //000000
4.运算符

![运算符和表达式](运算符和表达式.png)

	eg:计算时如果混合几种数据类型可能导致奇怪的结果，所以请谨慎地正确地引用你的变量，然后得出你期望的结果。
	比如输入 "35" + "25" 到控制台。为什么结果与你想象的不同？
	因为引号将数字转换成了字符串，所以最终会连接两个字符串而不是相加数字。如果你输入 35 + 25，你会得到正确的结果

5.语句

	eg:
	var iceCream = 'chocolate';
	
	if (iceCream === 'chocolate') {
	
	  alert('Yay, I love chocolate ice cream!');    
	} else {
	
	  alert('Awwww, but chocolate is my favorite...');    
	}

6.函数

functions是一种封装你想重复使用的功能的方法，可以在任何时候使用其中的功能，通过函数名称来调用而不是重复写完整代码

	eg：
	系统函数 alert(“这是一个警告框”)
	
	自定义函数
	function multiply(num1,num2) {
		  var result = num1 * num2;
		  return result;
	}
	var result = multiply(10,20);
	
	return 语句告诉浏览器返回 result 变量以便使用。这是很有必要的，因为函数内定义的变量只能在函数内使用。这叫做作用域 scoping
	
7.事件

有许多方法来将一个事件与元素绑定。在这里我们选择了 HTML 元素然后将 onclick 属性设置成包含单击时我们想要运行的代码的匿名函数.

	eg1：
	document.querySelector('html').onclick = function() {
	    alert('Ouch! Stop poking me!');
	}
	
	eg2:
	document.querySelector('html').onclick = function(){};
	
	eg3:
	var myHTML = document.querySelector('html');
	myHTML.onclick = function() {};
	
	
四、例子
1.点击图片改变图片

	var myImage = document.querySelector('img');
	myImage.onclick = function() {
   
    	var mySrc = myImage.getAttribute('src');
	   if(mySrc === 'images/firefox.png'){

	        myImage.setAttribute('src','images/firefox2.png');
   		 }else{

       	 myImage.setAttribute('src','images/firefox.png');
   		 }
	}

2.修改标题

	index.html：
	<body>
	
	     <button>Change user</button>
        <script src="scripts/main.js"></script>
    </body>

	main.js：

		var myButton = document.querySelector('button');
		var myHeading = document.querySelector('h1');

		function setUserName() {
  			var myName = prompt('Please enter your name.');
  			localStorage.setItem('name', myName);
  
 			 alert(myHeading.innerHTML);

  			myHeading.innerHTML = 'Mozilla is cool, ' + myName;
		}

		var storedName = localStorage.getItem('name');

		if(!storedName) {
  			setUserName();
		} else {
  
  			myHeading.innerHTML = 'Mozilla is cool, ' + storedName;
		}

		myButton.onclick = function() {
 			 setUserName();
		}
	
	
	
	
	
	
	
	
ps:

chrome浏览器控制台工具快捷打开方式：	
	
	1.command + option + i
	
参考资料：

1.[JavaScript基础](https://developer.mozilla.org/zh-CN/docs/Learn/Getting_started_with_the_web/JavaScript_basics)

2.[浏览器开发工具](https://developer.mozilla.org/zh-CN/docs/Learn/Discover_browser_developer_tools)

3.[变量名命名规则](http://www.codelifter.com/main/tips/tip_020.shtml)

4.[JavaScript指南](http://news.codecademy.com/your-guide-to-semicolons-in-javascript/)

5.[表达式和运算符](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators)

6.[变量作用域](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Grammar_and_types#Variable_scope)