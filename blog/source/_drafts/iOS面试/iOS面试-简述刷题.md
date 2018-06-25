---
title: iOS面试-简述刷题
date: 2018-06-05 11:37:11
tags: iOS面试
categories: iOS
---

1.iOS应用的导航模式
	
	平铺模式
		一般由UIScrollView和UIPageControl组合而成的展示方式;
		eg: 系统天气APP		
	标签模式
		tabBar的展示方式
		eg:系统时钟APP、App Store等
	树状模式
		tableView的多态展示方式
		eg:9宫格 - 照片App、系统邮件APP等
2.iOS中持久化方式

	属性列表
		NSUserDefaults 的存储，实际是本地生成一个 plist 文件，将所需属性存储在 plist 文件中
	对象归档
		本地创建文件并写入数据，文件类型不限
	SQLite数据库
		本地创建数据库文件，进行数据处理
	CoreData	
		同数据库处理思想相同，但实现方式不同
3.iOS单元测试框架
	
	OCUnit - OC官方测试框架，现在被XCTest取代
	XCTest - 与Foundation框架平行的测试框架
	GHUnit - 第三方测试框架
	OCMock - 第三方测试框架
4.iOS中OSI七层协议，TCP四层协议对应方式<br>
![OSI](OSI.png)<br>
5.iOS在项目中用过runtime吗?举个例子

	关联对象 Associated Objects
	消息发送 Messaging
	消息转发 Message Forwarding
	方法调配 Method Swizzling
	类对象 NSProxy Foundation 
	KVO、KVC 
	
	易忽略：
		动态获取class和slector
		给分类添加属性和KVO/KVC
	常用于：
		在类目中添加属性；
		修改系统方法如重写objectAtIndex方法防止数组越界；
6.iOS什么ARC?

7.iOS说明并比较关键字:atomatic、nonatomic

	atomatic
		提供多线程安全；
		描述变量是否支持多线程的同步访问；
		系统会自动的创建lock锁，锁定变量；
	nonatomic
		禁止多线程，变量保护，提高性能；
		
	atomatic和nonatomic
		用来决定编译器生成的getter和setter是否为原子操作；
	
8.iOS说明并比较关键字: __weak,_block

	__weak与weak基本相同：
		__weak 用于修饰变量(variable) 和 用于防止block中循环引用；
		weak 用于修饰属性(property)
	__block
		用于修饰变量；引用修饰，其修饰的值是动态变化的(可充新赋值)
		也可修饰某些block内部将要修改的外部变量；
	__weak和__block
		block是OC对与闭包(没有名字的函数或指向函数的指针)的实现；
9.iOS说明比较关键字：strong、weak、assign、copy
	
	strong
		指向并拥有对象，修饰对象引用计数+1，引用计数=0销毁，或强制销毁
		strong的复制是多个指针指向同一个地址
	weak
		指向不拥有对象，修饰对象引用计数 不加，对象会自行在内存中销毁
	assign
		修饰基本数据类型，主要存放于栈上，内存系统自动处理
		若修饰对象，释放后指针地址仍存在，造成野指针，堆上造成crash 
		CGFloat、NSInterger、int、float
	copy
		修饰有可变对应类型的不可变对象上，复制每次会在内存中拷贝一份对象，指针指向不同地址
		NSString、NSArray、NSDictionary
10.iOS中循环引用的情况

11.iOS 类-Class 和结构体-Struct
	
	
	
		
	



参考资料:<br>
1.[iOS面试大全](https://www.jianshu.com/p/1798ba01e9ef)<br>
2.[OCMock测试框架](https://github.com/erikdoe/ocmock)<br>
3.[GHUnit测试框架](https://github.com/gh-unit/gh-unit)<br>