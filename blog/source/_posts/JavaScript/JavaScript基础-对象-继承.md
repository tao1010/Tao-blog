---
title: JavaScript基础-对象-继承
date: 2018-04-18 14:57:34
tags: JavaScript
categories: Web
---

一、原型式继承	

<!--more-->
	
JavaScript继承的对象函数并不是通过复制而来，而是通过原型链继承（通常被称为 原型式继承 —— prototypal inheritance）     
1.例子	

```js
function Person(first, last, age, gender, interests) {
  this.name = {
    first,
    last
  };
  this.age = age;
  this.gender = gender;
  this.interests = interests;
};
//所以的方法都定义在构造器的原型上：
Person.prototype.bio = function() {
  alert('HI! How are you?');
};
Person.prototype.greeting = function() {
  alert('Hi! I\'m ' + this.name.first + '.');
};
Person.prototype.farewell = function() {
  alert(this.name.first + ' has left the building. Bye for now!');
}

```
2.定义构造函数		
创建一个Teacher类,继承与Person的所有成员:
	
	一个新的属性，subject - 教师教授的学科；
	一个被更新的greeting()方法；
有参数 - 构造函数继承:		
创建Teacher()构造器:

```js
//创建继承与Person的Teacher类
function Teacher(first,last,age,gender,interests,subject){

    /*允许调用一个在这个文件里别处定义的函数。
    第一个参数指明了在运行这个函数时想对“this”指定的值(可以重新指定您调用的函数里所有“this”指向的对象).
    其他的变量指明了所有目标函数运行时接受的参数.
    */
    Person.call(this,first,last,age,gender,interests);
    this.subject = subject;
}
```
无参数 - 构造函数继承:

```js
function Brick(){
	
	this.width = 10;
	this.height = 20;
}
//继承Brick()
function BlueGlassBrick(){
	
	Brick.call(this);
	this.opacity = 0.5;
	this.color = 'blue';
}
```
3.设置构造函数的原型和构造器引用		
继承方法:		
	
```js
Teacher.prototype = Object.create(Person.prototype);
Teacher.prototype.constructor = Teacher;
解析：
	create()创建一个和Person.prototype一样的新的原型属性值（这个属性指向一个包括属性和方法的对象），然后将其作为Teacher.prototype的属性值。这意味着Teacher.prototype现在会继承Person.prototype的所有属性和方法.
	生成Teacher()的方式决定了：Teacher()的prototype的constructor属性指向的是Person().所以修改constructor属性，改变其指向。
```
4.向构造器添加和重写新的函数		

```js
//新增方法：
Teacher.prototype.sayHi = function(){

    alert('Say Hi');
}
//重写方法：
Teacher.prototype.greeting = function(){
  var prefix;
  if(this.gender === 'male' || this.gender === 'Male' || this.gender === 'm' || this.gender === 'M') {
    prefix = 'Mr.';
  } else if(this.gender === 'female' || this.gender === 'Female' || this.gender === 'f' || this.gender === 'F') {
    prefix = 'Mrs.';
  } else {
    prefix = 'Mx.';
  }
  alert('Hello. My name is ' + prefix + ' ' + this.name.last + ', and I teach ' + this.subject + '.');
}
```
tips:此继承方法并不是唯一的方法.	
演示：	

```js
var teacher1 = new Teacher('Dave', 'Griffiths', 31, 'male', ['football', 'cookery'], 'mathematics');

控制台输入:
teacher1.name.first;
teacher1.interests[0];
teacher1.bio();
teacher1.subject;
teacher1.greeting();
```
练习：新增继承于Person()的Student类

```js
//创建继承Person的Student类
function Student(first,last,age,gender,interests,hello){

  Person.call(this,first,last,age,gender,interests);
  this.hello = hello;
}
//继承方法：
Student.prototype = Object.create(Person.prototype);
Student.prototype.constructor = Student;
//新增方法：
Student.prototype.sayHi = function(){

  alert('Hi,Gays!');
}

var student1 = new Student('Jim','Griffiths', 31, 'male', ['football', 'cookery']);
```
5.对象成员总结		

	那些定义在构造器函数中的、用于给予对象实例的。
		在代码中，它们是构造函数中使用this.x = x类型的行；
		在内置的浏览器代码中，它们是可用于对象实例的成员（通常通过使用new关键字调用构造函数来创建，例如var myInstance = new myConstructor()）。
	那些直接在构造函数上定义、仅在构造函数上可用的。
		这些通常仅在内置的浏览器对象中可用，
		并通过被直接链接到构造函数而不是实例来识别。 例如Object.keys()。
	那些在构造函数原型上定义、由所有实例和对象类继承的。
		这些包括在构造函数的原型属性上定义的任何成员，如myConstructor.prototype.x()。

6.JS中使用继承的场景			

	如果您开始创建一系列拥有相似特性的对象时，那么创建一个包含所有共有功能的通用对象，然后在更特殊的对象类型中继承这些特性，将会变得更加方便有用。
	如果您开始创建一系列拥有相似特性的对象时，那么创建一个包含所有共有功能的通用对象，然后在更特殊的对象类型中继承这些特性，将会变得更加方便有用。过多的继承会在调试代码时给您带来无尽的混乱和痛苦.

二、JSON的使用		
JSON是一种按照JavaScript对象语法的数据格式，用于将结构化数据表示为JavaScript对象的标准格式，通常用于再网站上表示和传输数据.
	
	从服务器向客户端发送一些数据，用于网页的显示.
	JSON可以作为一个对象或者字符串存在.
1.JSON结构		

```js
对象树:
{
  "squadName" : "Super hero squad",
  "homeTown" : "Metro City",
  "formed" : 2016,
  "secretBase" : "Super tower",
  "active" : true,
  "members" : [
    {
      "name" : "Molecule Man",
      "age" : 29,
      "secretIdentity" : "Dan Jukes",
      "powers" : [
        "Radiation resistance",
        "Turning tiny",
        "Radiation blast"
      ]
    },
    {
      "name" : "Madame Uppercut",
      "age" : 39,
      "secretIdentity" : "Jane Wilson",
      "powers" : [
        "Million tonne punch",
        "Damage resistance",
        "Superhuman reflexes"
      ]
    },
    {
      "name" : "Eternal Flame",
      "age" : 1000000,
      "secretIdentity" : "Unknown",
      "powers" : [
        "Immortality",
        "Heat Immunity",
        "Inferno",
        "Teleportation",
        "Interdimensional travel"
      ]
    }
  ]
}
解析：将上述对象保存为superHeroes对象
	访问对象内数据方法：. 或[]
		superHeroes.hometown
		superHeroes["active"]
	访问对象中的对象：链式访问(通过属性名和数组索引)
		superHeroes["members"][1]["powers"][2]
```
2.JSON数组		

```js
[
  {
    "name" : "Molecule Man",
    "age" : 29,
    "secretIdentity" : "Dan Jukes",
    "powers" : [
      "Radiation resistance",
      "Turning tiny",
      "Radiation blast"
    ]
  },
  {
    "name" : "Madame Uppercut",
    "age" : 39,
    "secretIdentity" : "Jane Wilson",
    "powers" : [
      "Million tonne punch",
      "Damage resistance",
      "Superhuman reflexes"
    ]
  }
]
解析:
	通过数组索引访问数组元素eg:[0]["powers"][0]
```
3.其他		

	JSON是一种纯数据格式，只包含属性，没有方法;
	JSON要求两头的{}来使其合法；
	JSON可以将任何标准合法的JSON数据格式化保存，不只是数组和对象；
	再JSON中，只有字符串才能用作属性；

4.完成例子:		
![json-superheroes](json-superheroes.png)     
加载json
	
```js
//1.声明变量 - 保存URL
var requestURL = 'https://mdn.github.io/learning-area/javascript/oojs/json/superheroes.json';

//2.创建HTTP请求
var request = new XMLHttpRequest();

//3.打开新的请求
request.open('GET', requestURL);

//4.设置XHR来访问JSON格式数据type,发送请求:
requset.responseType = 'json';
request.send();

//5.处理服务器回调数据
//onload函数只有在请求成功时触发，因此可以保证request.response是绝对可以访问的；
request.onload = function(){
	
	//存储返回数据
	var superHeroes = request.response;
	//定位header
	populateHeader(superHeroes);
	//创建英雄信息卡
		
}
```	
定位header
	
```js
//定位header
function populateHeader(jsonObj) {

  //创建变量 - h1
  var myH1 = document.createElement('h1');

  //写入json数据到Header
  myH1.textContent = jsonObj['squadName'];

  //添加到header对象中
  header.appendChild(myH1);

  var myPara = document.createElement('p');
  myPara.textContent = 'Hometown: ' + jsonObj['homeTown'] + ' // Formed: ' + jsonObj['formed'];
  header.appendChild(myPara);
}
```		
//创建英雄信息卡

```js
//创建英雄信息卡片
function showHeroes(jsonObj) {
  var heroes = jsonObj['members'];
      
  for(i = 0; i < heroes.length; i++) {
  
  	//创建元素
    var myArticle = document.createElement('article');
    var myH2 = document.createElement('h2');
    var myPara1 = document.createElement('p');
    var myPara2 = document.createElement('p');
    var myPara3 = document.createElement('p');
    var myList = document.createElement('ul');

    myH2.textContent = heroes[i].name;
    myPara1.textContent = 'Secret identity: ' + heroes[i].secretIdentity;
    myPara2.textContent = 'Age: ' + heroes[i].age;
    myPara3.textContent = 'Superpowers:';
        
    var superPowers = heroes[i].powers;
    for(j = 0; j < superPowers.length; j++) {
      var listItem = document.createElement('li');
      listItem.textContent = superPowers[j];
      myList.appendChild(listItem);
    }

    myArticle.appendChild(myH2);
    myArticle.appendChild(myPara1);
    myArticle.appendChild(myPara2);
    myArticle.appendChild(myPara3);
    myArticle.appendChild(myList);

    section.appendChild(myArticle);
  }
}
```
5.对象和文本间的转换		

	parse(): 以文本字符串形式接受JSON对象作为参数，并返回相应的对象。
	stringify(): 接收一个对象作为参数，返回一个对应的JSON字符串。

	var myJSON = { "name" : "Chris", "age" : "38" };
	myJSON
	var myString = JSON.stringify(myJSON);
	myString

参考资料：	
1.[对象](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Objects)		   
2.[JS中的继承](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Objects/Inheritance) 	
3.[JSON的使用](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Objects/JSON)