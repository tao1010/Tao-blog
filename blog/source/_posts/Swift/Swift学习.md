---
title: Swift教程
date: 2018-03-06 16:52:39
tags: swift
categories: iOS
---
version：4.0

	不需要main()函数；（全局作用域中的代码会被自动当做程序的入口点）
	
	不需要在每个语句结尾写上分号；

一、[基础部分](http://www.swift51.com/swift4.0/chapter2/01_The_Basics.html)

1.用 let 来声明常量，用 var 来声明变量
	
	常量的值一旦设定就不能改变，而变量的值可以随意更改.
	
	可以在一行中声明多个常量或者多个变量，用逗号隔开.
	
	常量与变量名不能包含数学符号，箭头，保留的（或者非法的）Unicode 码位，连线与制表符。
	也不能以数字开头，但是可以在常量与变量名的其他地方包含数字。
	
	(一般来说你很少需要写类型标注)当你声明常量或者变量的时候可以加上类型标注（type annotation），说明常量或者变量中要存储的值的类型.
	
eg:类型标注
	
``` swift
	var welcomeMessage:String//声明一个类型为 String ，名字为 welcomeMessage 的变量。
```

tips：

	如果你需要使用与Swift保留关键字相同的名称作为常量或者变量名，你可以使用反引号（`）将关键字包围的方式将其作为名字使用。无论如何，你应当避免使用关键字作为常量或变量名，除非你别无选择。

2.整数可以是 有符号（正、负、零）或者 无符号（正、零）

	let minValue = UInt8.min  // minValue 为 0，是 UInt8 类型
	let maxValue = UInt8.max  // maxValue 为 255，是 UInt8 类型



二、[基本运算符](http://www.swift51.com/swift4.0/chapter2/02_Basic_Operators.html)



3.[字符串和字符](http://www.swift51.com/swift4.0/chapter2/03_Strings_and_Characters.html)


4.[集合类型](http://www.swift51.com/swift4.0/chapter2/04_Collection_Types.html)

5.[控制流](http://www.swift51.com/swift4.0/chapter2/05_Control_Flow.html)


6.函数


7.闭包

8.枚举

9.类和结构体

10.属性

11.方法

12.下标

13.继承

14.构造过程和析构过程

15.自动引用计数

16.可选链

17.错误处理

18.类型转换

19.嵌套类型

20.扩展

21.协议

22.泛型

23.内存安全

24.访问控制

25.高级运算










参考资料：

1.[Swift编程](http://www.swift51.com/swift4.0/)
