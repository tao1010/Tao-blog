---
title: JavaScript基础-JS第一步-基础数据
date: 2018-04-16 11:51:57
tags: JavaScript
categories: Web
---

一、变量	
1.变量：就是用于存放数值的容器。

```html
<head>
  <meta charset="UTF-8">
  <link rel="stylesheet" href="js-demo.css">
</head>
<body>
  <div>
    <button>Press me</button>
  </div>
  <script src="js-demo.js"></script>
</body>
```
```js
var button = document.querySelector('button');
button.onclick = function(){

    var name = prompt('What is your name?');
    alert('Hello ' + name + ',nice to see you!');
}
```

	存放的数值可以改变;
	能够存储任何的东西 -- 不只是字符串和数字。变量可以存储更复杂的数据，甚至是函数;
	变量不是数值本身，仅仅是一个用于存储数值的容器;
2.声明变量: var ‘变量名’

	var name;
	var number;
	解析：当你使用它们的时候
		得到一个undefined的值,表示他们是空的容器；
		得到一个报错信息,'Uncaught referenceError：xxx is not defined',表示变量不存在，未定义；
	
tips:
	
	在JavaScript中，所有代码指令都会以分号结尾 (;) 
	如果忘记加分号，你的单行代码可能执行正常;
	但是在多行代码在一起的时候就可能出错.
3.初始化变量		
	
	方式一：
	var name;
	name = 'John';
	方式二：
	var name = 'John';
tips：
	
	如果你写一个声明和初始化变量的多行JavaScript代码的程序，你可以在初始化变量之后再实际声明它，并且它仍然可以工作。这是因为变量的声明通常在在其余的代码执行之前完成。这叫做顶置。

```js
function do_something() {
  console.log(bar); // undefined
  var bar = 111;
  console.log(bar); // 111
}

// is implicitly understood as: 
function do_something() {
  var bar;
  console.log(bar); // undefined
  bar = 111;
  console.log(bar); // 111
}
```
4.更新变量		
关于变量命名规则:

	使用拉丁字符(0-9,a-z,A-Z)和下划线字符;
	变量名不要以下划线开头—— 以下划线开头的被某些JavaScript设计为特殊的含义;
	变量名不要以数字开头;
	一个可靠的命名约定叫做 "小写驼峰命名法";(第一个单词小写，大写第二个单词之后的首字母);
	变量名大小写敏感(myage和myAge是2个不同的变量);
	避免使用JavaScript的保留字(关键字)给变量命名。如 var, function, let和 for等;
5.变量类型		
Number	
	
	var myMoney = 15.32;
	var myAge	= 12;
String	
	
	var myName = 'JiJi';	
	var myName = "JiJi";
Boolean		
	
	var iAmAlive = true;
	var isTest = 6 < 3;//返回false
Array		
	
	var myNameArray = ['Chris','Bob','Jim'];
	var maAgeArray = [10, 12, 43];
	var allArray = [10, 12, 'Bob'];
<font color='red'>tips:JavaScript的弱数据类型决定数组中元素类型可以不同的;</font>    
Object			
对象是现实生活中的模型的一种代码结构;

	var dog = { name : 'Spot', breed : 'Dalmatian' };
要检索存储在对象中的信息，可以使用以下语法:
	
	dog.name
6.松散类型			
JavaScript是一种“松散类型语言”，这意味着不同于其他一些语言(译者注：如C、JAVA)，您不需要指定变量将包含什么数据类型（例如number或string）	

	var myNumber = '500'; // oops, this is still a string
	typeof(myNumber);
	myNumber = 500; // much better — now this is a number
	typeof(myNumber)
二、数字和操作符		
1.数字类型			
	
	在JavaScript中有一个称为typeof的运算符;
2.算数运算符
	
	算数优先级
3.递增和递减运算符		
4.操作运算符	
5.比较运算符		

	==和!= ，测试值是否相同， 但是数据类型可能不同，
	===/!==，严格版本测试值和数据类型是否相同。 
	严格的版本往往导致更少的错误，所以我们建议您使用这些严格的版本。
三、字符串		
1.字符串的创建;		
2.单引号和双引号的使用,不能混合使用;		
3.转义字符串中的字符;
	
	var bigmouth = 'I\'ve got no right to take my place...';
	bigmouth;
4.连接字符串(+);		
5.上下文的串联;		

```html
<head>
  <meta charset="UTF-8">
  <link rel="stylesheet" href="js-demo.css">
</head>
<body>
  <div>
    <button>Press me</button>
  </div>
  <script src="js-demo.js"></script>
</body>
```
```js
var button = document.querySelector('button');
button.onclick = function(){

    var name = prompt('What is your name?');
    alert('Hello ' + name + ',nice to see you!');
}
```
6.数字与字符		
数字转字符串：
	
	var myNum = 123;
	var myString = myNum.toString();
	typeof myString;
字符串转数字:

	var myString = '123';
	var myNum = Number(myString);
	typeof myNum;
四、字符串方法		
1.把字符串当对象			
	
	var string = 'This is my string';
	
	获取字符串长度：
	string.length;
	
	检索特定字符串字符：
	string[2];//获取字符串中第三个字符
	
	在字符串中查找子字符串并提取它:
	string.indexOf('my'); //返回m对应的下标位置，如果没有检测到返回-1
	string.slice(0,3);//提取字符串中的0-3的字符
	string.slice(2);//提取从第二个字符开始的所有字符

	转换事例
	string.toLowerCase();//所以字符转成小写
	string.toUpperCase();//所以字符转成大写
	
	更新字符串的部分
	string.replace('is','are');//将字符串中is替换成are
五、数组		
1.数组概念：通常被描述为“列表样对象”; 它们基本上是包含存储在列表中的多个值的单个对象。 数组对象可以存储在变量中并以与其他任何类型的值完全相同的方式处理，区别在于我们可以单独访问列表中的每个值，并使用列表执行超级有用和高效的操作。		
2.创建数组:数组由方括号构成，其中包含用逗号分隔的项目列表.   

	var shopping = ['bread', 'milk', 'cheese'];
	var sequence = [1, 1, 2, 3, 5, 8, 13];
	var random = ['tree', 795, [0, 1, 2]];

	可以将数组中的任何项目存储在数组中 - 字符串，数字，对象，另一个变量，甚至另一个数组;
3.修改和访问数组		
	
	var shopping = ['bread', 'milk', 'cheese', 'hummus', 'noodles'];

	数组长度|元素个数
	shopping.length;
4.数组方法		

	var myData = 'Manchester,London,Liverpool,Carlisle';
	
	以‘,’分割字符串为数组：
	var myArray = myData.split(',');
	
	以‘,’拼接数组元素为字符串：
	1> var myNewString = myArray.join(',');
	2> var myString = myArray.toString();

	向数组末尾中添加元素(一个或多个):
	myArray.push('addNew');
	myArray.push('addNew1','addNew2');
	
	删除数组中最后一个元素：
	var removedItem = myArray.pop();
	
	unshift()和shift()以完全相同的方式工作，只是它们在数组的开始处，而不是结尾。
	myArray.unshift('Edinburgh');//添加元素到数组首位
	var removedItem = myArray.shift();
六、应用 - 故事生成器		
![example](example.png)

```html
<head>
  <meta charset="UTF-8">
  <link rel="stylesheet" href="js-demo.css">
</head>
<body>

    <div>
        <label for="customname">Enter custom name:</label>
        <input id="customname" type="text" placeholder="">
      </div>
      <div>
        <label for="us">US</label><input id="us" type="radio" name="ukus" value="us" checked>
        <label for="uk">UK</label><input id="uk" type="radio" name="ukus" value="uk">
      </div>
      <div>
        <button class="randomize">Generate random story</button>
      </div>
      <!-- Thanks a lot to Willy Aguirre for his help with the code for this assessment -->
      <p class="story"></p>
  <script src="js-demo.js"></script>
</body>
```
```css
var button = document.querySelector('.randomize');
var customName = document.getElementById('customname');
var story = document.querySelector('.story');

function randomValueFromArray(array){
  return array[Math.floor(Math.random()*array.length)];
}

var storyText = 'It was 94 farenheit outside, so :insertx: went for a walk. When they got to :inserty:, they stared in horror for a few moments, then :insertz:. Bob saw the whole thing, but was not surprised — :insertx: weighs 300 pounds, and it was a hot day.';
var insertX = ['Willy the Goblin','Big Daddy','Father Christmas'];
var insertY = ['the soup kitchen','Disneyland','the White House'];
var insertZ = ['spontaneously combusted','melted into a puddle on the sidewalk','turned into a slug and crawled away'];

button.addEventListener('click', generateStroy);

function generateStroy(){

  var newStroy = storyText;

  var xItem = randomValueFromArray(insertX);
  var yItem = randomValueFromArray(insertY);
  var zItem = randomValueFromArray(insertZ);

  newStroy = newStroy.replace(':insertx:', xItem);
  newStroy = newStroy.replace(':insertx:', xItem);

  newStroy = newStroy.replace(':inserty:', yItem);
  newStroy = newStroy.replace(':insertz:', zItem);

  if(customName.value !== '') {
    var name = customName.value;
    newStroy = newStroy.replace('Bob', name);
  }

  if(document.getElementById("uk").checked) {

    // var weight = Math.round(300);
    // var temperature =  Math.round(94);
    var weight = Math.round(300*0.0714286) + ' stone';//Math.round() 是 JavaScript 中的内建函数，用来生成最接近一个计算式的整数
    var temperature =  Math.round((94-32) * 5 / 9) + ' centigrade';
    newStory = newStory.replace('94 farenheit',temperature);
    newStory = newStory.replace('300 pounds',weight);

  }

  story.textContent = newStroy;
  story.style.visibility = 'visible';
}
```






	
参考资料：	
1.[变量](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/First_steps/Variables)		
2.[顶置](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/var#var_hoisting)    
3.[数字和操作符](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/First_steps/Math)	   
4.[转义字符](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String#Escape_notation)  
5.[数组](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/First_steps/Arrays)
6.[应用](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/First_steps/Silly_story_generator)