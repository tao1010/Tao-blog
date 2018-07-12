---
title: iOS其他-其他
toc: true
date: 2017-03-04 15:50:12
tags: iOS其他
categories: iOS
---

1.忽略警告

<!-- more -->

	#pragma clang diagnostic push
	#pragma clang diagnostic ignored "-Wgnu"
	#pragma clang diagnostic pop
	
	解析：忽略 ？: 条件表达式带来的警告
	
	#pragma clang diagnostic push
	#pragma clang diagnostic ignored "-Wdeprecated-declarations"
	#pragma clang diagnostic pop

	解析：忽略方法弃用的警告
2.initWithFrame、initWithCoder、awakeFromNib区别
	
	initWithFrame
		当控件不是从xib、storyboard中创建时，会调用这个方法;
		在定义控件内,如果 调用initWithFrame方法，其余2个方法不会调用;
		适用纯代码创建时调用，不涉及xib或storyboard;
	initWithCoder
		当控件是从xib、storyboard中创建时，会调用这个方法;
	awakeFromNib
		在initWithCoder:方法后调用 ，顺序是：initWithCoder  -> awakeFromNib
	
	initWithCoder、awakeFromNib是从xib、storyboard中创建时会调用;

参考资料:<br>
1.[忽略警告的方法](https://www.jianshu.com/p/9fbb9b8ed5fb)     
2.[各种编译器警告](http://fuckingclangwarnings.com/#semantic)     
3.[initWith...区别](https://www.cnblogs.com/yajunLi/p/6344023.html)