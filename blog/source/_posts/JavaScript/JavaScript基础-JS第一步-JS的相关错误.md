---
title: JavaScript基础-JS第一步-JS的相关错误
date: 2018-04-16 10:57:32
tags: JavaScript
categories: Web
---

一、错误类型		
语法错误：
	
	代码的拼写错误，实际上导致程序不能运行在所有或停止通过工作的一部分，这样您通常会用一些提供的错误消息找到修复的方法，只要您熟悉正确的工具，知道错误消息的意思！
逻辑错误：
	
	这些错误，其中语法实际上是正确的，但代码是不是你想要的，这意味着项目成功运行，但会产生不正确的结果。这些通常比语法错误更难以修复，因为通常没有错误指向错误源。

二、修复语法错误		
1.JavaScript是区分大小写的，所以任何轻微的拼写或大小不同都会导致错误；
	
	eg:
	guessSubmit.addeventListener('click', checkGuess);
			
2.在控制台(网页的右键->检查）工具中，console.log()是一个非常有用的调试功能 - 打印的值到控制台

```html
 <div class="resultParas">
      <p class="guesses"></p>
      <p class="lastResult"></p>
      <p class="lowOrHi"></p>
    </div>
    
```
	var lowOrHi = document.querySelector('lowOrHi');
	修改：
	var lowOrHi = document.querySelector('.lowOrHi');
三、逻辑错误			
1.代码逻辑错误
	
	var randomNumber = Math.floor(Math.random()) + 1;
	//随机数总是为1；
	修改成：
	var randomNumber = Math.floor(Math.random() * 100) + 1;
	解析：
		Math.random() 生成一个在0和1之间的十进制随机数;
		Math.floor()  向下舍入到与它最接近的整数;
四、其他常见错误		
1.语法错误：在声明后缺少‘;’	
2.混淆赋值和严格相等运算符

	eg： === 和 = 
3.语法错误：在参数后缺少')'	
	
	function checkGuess({}) - 错误
	
	function checkGuess(){} - 正确
	
4.语法错误:在属性id后缺少':'		
5.语法错误:在函数体后缺少'}'	

	这个错误通常意味着你丢失了字符串值的开引号或关引号;
	

参考资料：		
1.[JavaScript的相关错误](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/First_steps/What_went_wrong)		
2.[类型错误](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Errors/Unexpected_type)   
3.[错误文档](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Errors)		