---
title: JavaScript基础-对象-对象原型
date: 2018-04-18 13:30:23
tags: JavaScript
categories: Web
---

一、对象原型	

<!--more-->

1.JavaScript 常被描述为一种基于原型的语言 (prototype-based language)
	
	每个对象拥有一个原型对象，对象以其原型为模板、从原型继承方法和属性。		
	原型对象也可能拥有原型，并从中继承方法和属性，一层一层、以此类推。这种关系常被称为原型链 (prototype chain)，它解释了为何一个对象会拥有定义在其他对象中的属性和方法。
	属性和方法定义在Object的构造器函数(constructor functions)之上的prototype属性上，而非对象实例本身。	
理解：
	
	对象的原型（可以通过Object.getPrototypeOf(obj)或者已被弃用的__proto__属性获得） - 每个实例上都有的属性；
	构造函数的prototype属性之间的区别是很重要的。- 构造函数的属性。
	也就是说，Object.getPrototypeOf(new Foobar())和Foobar.prototype指向着同一个对象。
2.例子：

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

     var genderString = 'He'; 
     if(gender !== 'male'){

        genderString = 'She';
     }
     var message = this.name.first + ' ' + this.name.last + ' is ' + this.age + ' years old. ' + genderString + ' likes ';
     for(i = 0; i < interests.length; i ++){

        if(i == interests.length - 1){

          message += ' and ' + interests[i];
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
  this.greeting = function(){

    alert('Hi! I\'m ' + this.name.first + '.');
  }
}

var person1 = new Person('Bob', 'Smith', 32,'male',['music','shiing','cooking']);
```	
```js
控制台输入：

查看定义的person1的原型对象(Person()构造器中的成员:this指向的所有属性):
	person1.

调用方法：
person1.valueOf()
	
	浏览器检查person1对象是否具有可用的valueof()方法；
		false:检查person1对象的原型对象(Person)是否具有可用的valueof()方法；
			false:检查 Person() 构造器的原型对象（即 Object）是否具有可用的 valueOf() 方法。Object 具有这个方法，于是该方法被调用;
```
<font color='red'>注意:</font>		
	
	原型链中的方法和属性没有被复制到其他对象——它们被访问需要通过前面所说的“原型链”的方式;
	没有官方的方法用于直接访问一个对象的原型对象——原型链中的“连接”被定义在一个内部属性中，在 JavaScript 语言标准中用 [[prototype]] 表示;
	大多数现代浏览器还是提供了一个名为 __proto__ （前后各有2个下划线）的属性，其包含了对象的原型。（验证：person1.__proto__ 和 person1.__proto__.__proto__）
</font>	
3.prototype属性：继承成员被定义的地方		
<font color='blue'>重要：</font>prototype 属性大概是 JavaScript 中最容易混淆的名称之一。你可能会认为，这个属性指向当前对象的原型对象，其实不是（还记得么？原型对象是一个内部对象，应当使用 __proto__ 访问）。prototype 属性包含（指向）一个对象，你在这个对象中定义需要被继承的成员.		
4.create() - 创建新的对象实例		

	控制台输入:
	var person2 = Object.create(person1);
	create()实际做的是从指定原型对象创建一个新的对象。这里以 person1 为原型对象创建了 person2 对象。
	在控制台输入：
	person2.__proto__
5.constructor属性		
每个对象实例都具有 constructor 属性，它指向创建该实例的构造器函数.			
	
	//控制台输入：
	
	//返回 Person() 构造器，因为该构造器包含这些实例的原始定义
	person1.constructor
	
	//获得某个对象实例的构造器的名字
	person1.constructor.name
	
可以在 constructor 属性的末尾添加一对圆括号（括号中包含所需的参数），从而用这个构造器创建另一个对象实例。毕竟构造器是一个函数，故可以通过圆括号调用；只需在前面添加 new 关键字，便能将此函数作为构造器使用:
	
	//控制台输入:
	var person3 = new person1.constructor('Karen', 'Stephenson', 26, 'female', ['playing drums', 'mountain climbing']);

	person3.name.first
	person3.age
	person3.bio()
6.修改原型
	
	为构造器的prototype属性添加一个新的方法：
	
	function Person(first, last, age, gender, interests) {
	  // 属性与方法定义
	  ...
	};
	var person1 = new Person('Tammi', 'Smith', 32, 'neutral', ['music', 'skiing', 'kickboxing']);
	
	Person.prototype.farewell = function() {
	  alert(this.name.first + ' has left the building. Bye for now!');
	}
	
	控制台输入：
	person1.farewell();
关键的是，整条继承链动态地更新了，任何由此构造器创建的对象实例都自动获得了这个方法。

	//添加一个属性:
	Person.prototype.fullName = 'Bob Smith';
	
	//无效的，因为本例中 this 引用全局范围，而非函数范围。访问这个属性只会得到 undefined undefined。但这个语句若放在先前定义的 prototype 的方法中则有效，因为此时语句位于函数范围内，从而能够成功地转换为对象实例范围：
	Person.prototype.fullName = this.name.first + ' ' + this.name.last;
	
	
构造器只包含属性，方法则分装在不同的代码块中，使其更具可读性：
	
	// 构造器及其属性定义
	function Test(a,b,c,d) {
	  // 属性定义
	};
	
	// 定义第一个方法
	
	Test.prototype.x = function () { ... }
	
	// 定义第二个方法
	
	Test.prototype.y = function () { ... }
	
	// 等等……
			
参考资料：	
1.[对象](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Objects)		   
2.[对象原型](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Objects/Object_prototypes)         
3.[Object对象](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object)    


