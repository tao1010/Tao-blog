---
title: JavaScript基础-JS第一步-基础例子
date: 2018-04-11 15:27:21
tags: JavaScript
categories: Web

---

一、猜数字游戏	

<!--more-->

1.需求

	在1~100以内随机选择一个数, 然后让玩家挑战在10轮以内猜出这个数字，每一轮都要告诉玩家正确或者错误， 如果出错了，则告诉他数字是低了还是高了，并且还要告诉玩家之前猜的数字是什么。 一旦玩家猜测正确，或者他们用完了回合，游戏将结束。 游戏结束后，可以让玩家选择再次开始。
2.流程	
	
	生成1到100之间的随机数。
	记录玩家在第几轮。从1开始。
	为玩家提供一种猜测数字的方法。
	一旦提交了猜测，首先将它记录在某处，以便用户可以看到他们先前的猜测。
	接下来检查它是否是正确的数字。
	如果是正确的：
			显示祝贺消息。
			阻止玩家输入更多的猜测（这会使游戏混乱）。
			显示控制允许玩家重新开始游戏。
	如果它错了，并且玩家有剩余轮次：
			告诉玩家他们错了。
			允许他们输入另一个猜测。
			将圈数增加1。
	如果它是错误的，并且玩家没有剩余轮次：
			告诉玩家游戏结束。
			阻止玩家输入更多的猜测（这会使游戏混乱）。
			显示控制允许玩家重新开始游戏。
	 一旦游戏重新启动，请确保游戏逻辑和用户界面完全重置，然后返回步骤1。
3.初始设置

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <link rel="stylesheet" href="js-demo.css">
</head>
<body>

    <h1>Number guessing game</h1>

    <p>We have selected a random number between 1 and 100. See if you can guess it in 10 turns or fewer. We'll tell you if your guess was too high or too low.</p>

    <div class="form">
      <label for="guessField">Enter a guess: </label><input type="text" id="guessField" class="guessField">
      <input type="submit" value="Submit guess" class="guessSubmit">
    </div>

    <div class="resultParas">
      <p class="guesses"></p>
      <p class="lastResult"></p>
      <p class="lowOrHi"></p>
    </div>

  <script src="js-demo.js"></script>
</body>
</html>
```
4.添加变量以保存数据		
在script中添加如下变量：

```js
var randomNumber = Math.floor(Math.random() * 100) + 1;

var guesses = document.querySelector('.guesses');
var lastResult = document.querySelector('.lastResult');
var lowOrHi = document.querySelector('.lowOrHi');

var guessSubmit = document.querySelector('.guessSubmit');
var guessField = document.querySelector('.guessField');

var guessCount = 1;
var resetButton;
```
	
	变量基本上是值（例如数字或文本字符串）的容器;
	使用关键字var以及变量的名称创建一个变量;
	使用等号（=）和要赋予的值为变量赋值;
5.函数(function)		
在&lt;script&gt;中写入函数：

``` js
function checkGuess() {
  alert('I am a placeholder');
}
```
解析：
	
	函数是可重复使用的代码块，可以“编写一次，到处运行”，从而节省了大量的重复代码；
	使用关键字function定义了一个函数，后面跟着一个名字，再后面加了括号。 之后，我们放两个大括号（{}）。 在大括号里面，放置着当我们调用该函数时所有想要运行的代码。
6.运算符(operators)		
JavaScript运算符允许我们执行比较，做数学运算，连接字符串，以及其他类似的事情.		
+ 运算符：可用于字符串的连接
	
```js
var name = 'Bingo';
name;
var hello = ' says hello!';
hello;
var greeting = name + hello;
greeting;
```	 		
+= 增强赋值操作符

```js
name += ' says hello!';
等价于
name = name + ' says hello!';
```
算术运算符	

|运算符|名称|示例|
|---|---|---|
|+|||
|-|||
|*|||
|/|||

比较运算符

|运算符|名称|示例|
|---|---|---|
|===|严格运算符(它们是否确实一样?)|5 === 2 + 4|
|!==|严格不等运算符(它们不一样?)|'Chris' !== 'Ch' + 'ris'|
|>|||
|<|||
7.条件(conditionals)			
完善函数

```js
function checkGuess() {
	
	//用户输入的值   Number()-方法确保输入的是一个数字
  var userGuess = Number(guessField.value);
  
  if (guessCount === 1) {
    //玩家第一次猜数字：显示文本内容
    guesses.textContent = 'Previous guesses: ';
  }
 
  guesses.textContent += userGuess + ' '; //字符串拼接
 
 //猜测数字 =？随机数
  if (userGuess === randomNumber) {
  
  	//显示最终结果文本
    lastResult.textContent = 'Congratulations! You got it right!';
    lastResult.style.backgroundColor = 'green';//设置颜色
    lowOrHi.textContent = '';//清空提示信息
    setGameOver();//调用结束方法
    
  } else if (guessCount === 10) {
  	//超过猜测次数，提示文本，调用结束方法
    lastResult.textContent = '!!!GAME OVER!!!';
    setGameOver();
    
  } else {
  	
  	//猜测错误和猜测10次以内的情况
  	//猜测状态提示，结果提示
    lastResult.textContent = 'Wrong!';
    lastResult.style.backgroundColor = 'red';

    if(userGuess < randomNumber) {

      lowOrHi.textContent = 'Last guess was too low!';
    } else if(userGuess > randomNumber) {

      lowOrHi.textContent = 'Last guess was too high!';
    }
  }
 
 //为下一次猜测做准备
  guessCount++;//次数+1
  guessField.value = '';//文本清空
  guessField.focus();//重置输入框焦点
}
```
8.事件(Events)		
事件监听器:侦听事件发生的构造方法；		
事件处理器:响应事件触发而运行的代码块；		

```js
//在checkGuess()函数的结束大括号后添加一下代码：

guessSubmit.addEventListener('click', checkGuess);

解析：
	为guessSubmit按钮添加一个监听事件，这个方法包含两个可输入值（参数），监听事件的类型（在本例中为“点击”），和当事件发生时我们想要执行的代码（在本例中为checkGuess()函数）——注意，当函数作为事件监听方法的参数时，函数名后不应带括号。
```
9.完善游戏 - 处理后续逻辑		
添加函数setGameOver():				

```js
function setGameOver(){
	
	//禁用表单文本输入和按钮，方法是将其disable属性设置为true
    guessField.disabled = true;
    guessSubmit.diableed = true;
    
    //创建了一个新的button元素，设置它的文本为“Start new game”,并把它添加到我们文档的底部
    resetButton = document.createElement('button');
    resetButton.textContent = 'Start new game';
    document.body.appendChild(resetButton);
    
    //设置事件监听器，当它被点击时，一个名为resetGame()的函数被将被调用；
    resetButton.addEventListener('click',resetGame);
  }
  
function resetGame(){

	//重置猜测次数
    guessCount = 1;
    
    //清除信息段落
    var resetParas = document.querySelectorAll('.resultParas p');
    for(var i = 0; i < resetParas.length; i ++){

        resetParas[i].textContent = '';
    }
    //删除重置按钮
    resetButton.parentNode.removeChild(resetButton);
    
    //启动表单元素，清空文本，聚焦文本，
    guessField.disabled = false;
    guessSubmit.disabled = false;
    guessField.value = '';
    guessField.focus();
	
	//删除结果文本显示框的背景颜色
    lastResult.style.backgroundColor = 'white';
    //生成新的随机数
    randomNumber = Math.floor(Math.random() * 100) + 1;
  }
  
```
![game](game.png)		
10.循环(Loops)

```html
  <div class="resultParas">
      <p class="guesses"></p>
      <p class="lastResult"></p>
      <p class="lowOrHi"></p>
    </div>
    
```
```js
//清除信息段落
    var resetParas = document.querySelectorAll('.resultParas p');
    for(var i = 0; i < resetParas.length; i ++){

        resetParas[i].textContent = '';
    }
 解析：
 	创建一个resetParas变量，包含一个列表中的所有段落，然后依次通过每个段落，删除每个段落的文本内容。	
```
11.一些系统函数			
自动放置文本光标在输入框内:
	
	guessField.focus();
在JavaScript中,一切都是对象，对象是存储在单个分组中的相关功能的集合。可以创建自己的对象(暂不涉及)，浏览器包含的内置对象，允许你的操作：
	
	var guessField = document.querySelector('.guessField');
	解析：
		使用了document对象的querySelector()方法来获得此引用。querySelector()需要一个参数 — — 用该元素的 CSS 选择器选择您想要的引用的元素
12.操作浏览器对象		

在JavaScript中可修改Html的值;

使用JavaScript在元素上动态设置新的CSS样式；

	页面上的每个元素都有一个style属性，它本身包含一个对象，其属性包含应用于该元素的所有内联CSS样式。

参考资料：			
1.[JavaScript第一课](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/First_steps/A_first_splash)	