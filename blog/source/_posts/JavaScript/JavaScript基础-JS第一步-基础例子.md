---
title: JavaScript基础-JS第一步-基础例子
date: 2018-04-11 15:27:21
tags: JavaScript
categories: Web

---

一、猜数字游戏	

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
  var userGuess = Number(guessField.value);
  if (guessCount === 1) {
    guesses.textContent = 'Previous guesses: ';
  }
  guesses.textContent += userGuess + ' ';
 
  if (userGuess === randomNumber) {
    lastResult.textContent = 'Congratulations! You got it right!';
    lastResult.style.backgroundColor = 'green';
    lowOrHi.textContent = '';
    setGameOver();
  } else if (guessCount === 10) {
    lastResult.textContent = '!!!GAME OVER!!!';
    setGameOver();
  } else {
    lastResult.textContent = 'Wrong!';
    lastResult.style.backgroundColor = 'red';
    if(userGuess < randomNumber) {
      lowOrHi.textContent = 'Last guess was too low!';
    } else if(userGuess > randomNumber) {
      lowOrHi.textContent = 'Last guess was too high!';
    }
  }
 
  guessCount++;
  guessField.value = '';
  guessField.focus();
}
```



参考资料：			
1.[JavaScript第一课](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/First_steps/A_first_splash)		
2.[]()		
3.[]()		