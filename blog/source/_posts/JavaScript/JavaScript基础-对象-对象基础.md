---
title: JavaScript基础-对象-对象基础
date: 2018-04-18 09:11:08
tags: JavaScript
categories: Web
---

在JS中大多数事物都是对象(字符串、数字、数组、函数、API等),面向对象(object oriented)-OO

一、对象基础		
1.对象:是一个包含相关数据和方法的集合（通常由一些变量和函数组成，我们称之为对象里面的属性和方法.		

```js
语法：
var objectName = {
  member1Name : member1Value,
  member2Name : member2Value,
  member3Name : member3Value
}
解析：
	一个对象由许多的成员组成；
	每一个成员都拥有一个名字（像上面的name、age），和一个值（如['Bob', 'Smith']、32）；
	每一个名字/值（name/value）对被逗号分隔开；
	名字和值之间由冒号（:）分隔；
eg:
var person = {
  name:['Bob', 'Smith'],
  age:32,
  gender:'male',
  interests:['music','skiing'],
  bio:function(){
      alert(this.name[0] + ' ' + this.name[1] + ' is ' + this.age + ' years old.He likes ' + this.interests[0] + ' and ' + this.interests[1] + '.');
  },
  greeting:function(){

    alert('Hi! I\'m' + this.name[0] + '.');
  }

};
```
```js
控制台输入：
person.name[0]
person.age
person.interests[1]
person.bio()
person.greeting()
```
2.字面量：手动的写出对象的内容来创建一个对象。不同于从类实例化一个对象。（如上述eg的对象）当你想要传输一些有结构和关联的资料时常见的方式是使用字面量来创建一个对象，（eg:发起请求到服务器存储数据到数据库）		

3.访问属性的方式 - 点表示法	

	person.age
	person.interests[1]
	person.bio()
子命名空间
	
	name:['Bob','Smith'],

	person.name[0]
	person.name[1]
	改成： 链式
	name:{
		first: 'Bob',
		last: 'Smith'
	}
	person.name.first
	person.name.last
4.访问属性的方式 - 括号表示法	

	person.age
	person.name.first
	改成：
	person['age']
	person['name']['first']
	
	可以动态的去设置对象成员的值，还可以动态的去设置成员的名字.
5.设置对象成员		
修改值：	

	person.age = 21
	person.name[0] = 'John'
新增：
	
	person['eyes'] = 'hazel'
	person.farewell = function(){
		alert('Bye everybody!');
	}
6.this的含义		

	greeting: function() {
	  alert('Hi! I\'m ' + this.name.first + '.');
	}
	关键字"this"：
		指向了当前代码运行时的对象；
		保证了当代码的上下文(context)改变时变量的值的正确性；
		在字面量的对象里this看起来不是很有用，但是当你动态创建一个对象（例如使用构造器）时它是非常有用的；
7.常用的使用对象场景		

字符串对象：
	
	myString.split(',');
访问document对象:

	var myDiv = document.createElement('div');
	var myVideo = document.querySelector('video');
内建的对象或API不会总是自动地创建对象的实例：
	
	 Notifications API——允许浏览器发起系统通知，需要你为每一个你想发起的通知都使用构造器进行实例化：
	var myNotification = new Notification('Hello!');
二、构建函数和对象实例			
1.在一些面向对象的语言中，我们用类（class）的概念去描述一个对象（JavaScript使用了一个完全不同的术语）-类并不完全是一个对象，它更像是一个定义对象特质的模板。		
2.多态——是用来描述多个对象拥有实现共同方法的能力。     
3.JavaScript 用一种称为构建函数的特殊函数来定义对象和它们的特征；			
	构建函数提供了创建您所需对象（实例）的有效方法，将对象的数据和特征函数按需联结至相应对象;	
4.例子		
eg1:		

```js
function createNewPerson(name) {
	var obj = {};
	obj.name = name;
	obj.greeting = function() {
		alert('Hi! I\'m' + this.name + '.');
	}
	return obj;
}

控制台console:
var salva = createNewPerson('salva');
salva.name;
salva.greeting();

解析：
	代码有点冗长，如果我们知道如何创建一个对象，就没有必要创建一个新的空对象并且返回它；
```
eg2:一个构建函数通常是大写字母开头，便于区分构建函数和普通函数	

```js
function Person(name){
	
	this.name = name;
	this.greeting = function(){
		
		alert('Hi! I\'m' + this.name + '.');
	}
}
控制台输入：
var person1 = new Person('Bob');
var person2 = new Person('Sarah');
person1.name
person1.greeting()
person2.name
person2.greeting()

解析：
	只定义了对象的属性和方法，没有明确创建一个对象和返回任何值
	使用了this关键词，即无论是该对象的哪个实例被这个构建函数创建，它的 name 属性就是传递到构建函数形参name的值，它的 greeting() 方法中也将使用相同的传递到构建函数形参name的值；
```
5.创建构造函数		

```js

function Person(first, last, age, gender, interests){

  this.name = {

    first,
    last
  };
  this.age = age;
  this.gender = gender;
  this.interests = interests;
  this.bio = function (){

    alert(this.name.first + ' ' + this.name.last + ' is ' + this.age + ' years old. He likes ' + this.interests[0] + ' and ' + this.interests[1] + '.');
  };
  this.greeting = function(){

    alert('Hi! I\'m ' + this.name.first + '.');
  }
}

控制台输入：
//创建实例对象
var person1 = new Person('Bob', 'Smith', 32, 'male', ['music', 'skiing']);

person1['age']
person1.interests[1]
person1.bio()
解析：
	bio()方法 - 创建的Person是女性，或者是些别的性别类型，输出里的代词都总是 "He"；
	有更多的兴趣列举在interests数组中， bio只会展示两个兴趣；
优化：
this.bio = function (){

 var genderString = 'He'; 
 if(gender !== 'male'){

    genderString = 'She';
 }
 var message = this.name.first + ' ' + this.name.last + ' is ' + this.age + ' years old. ' + genderString + ' likes ';
 for(i = 0; i < interests.length; i ++){

    if(i == interests.length - 1){

			
      message += 'and ' + interests[i];
    }else{

      if(interests.length >= 2 && 
        i == interests.length - 2){

          message += interests[i];
      }else{
          message += interests[i] + ',';
      }
    }
 }
 message += '.';

alert(message);
};
```	
6.创建对象实例的其他方式		
Object()构造函数：
	
	//创建一个空的对象
	var person = new Object();	
	
	//写入数据(点语法+括号方法)
	person1.name = 'Chris';
	person1['age'] = 38;
	person1.greeting = function() {
	  alert('Hi! I\'m ' + this.name + '.');
	}
	
	//将对象文本传递给Object()构造函数作为参数， 以便用属性/方法填充它
	var person1 = new Object({
	  name : 'Chris',
	  age : 38,
	  greeting : function() {
	    alert('Hi! I\'m ' + this.name + '.');
	  }
	});
使用create()方法：
	
	JavaScript有个内嵌的方法create(), 它允许您基于现有对象创建新的对象实例。
	var person2 = Object.create(person1);
	
	//写入数据
	person2.name
	person2.greeting()
	
	解析：
		person2是基于person1创建的， 它们具有相同的属性和方法；
		它允许您创建新的对象实例而无需定义构造函数；
		缺点是比起构造函数，浏览器在更晚的时候才支持create()方法（IE9,  IE8 或甚至以前相比);

参考资料：	
1.[对象](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Objects)		
2.[对象基础](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Objects/Basics)		
3.[构建函数和对象实例](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Objects/Object-oriented_JS)    
	