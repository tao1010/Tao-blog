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
5.
参考资料:<br>
1.[iOS面试大全](https://www.jianshu.com/p/1798ba01e9ef)<br>
2.[OCMock测试框架](https://github.com/erikdoe/ocmock)<br>
3.[GHUnit测试框架](https://github.com/gh-unit/gh-unit)<br>