---
title: iOS其他-其他
toc: true
date: 2018-07-04 15:50:12
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

参考资料:<br>
1.[忽略警告的方法](https://www.jianshu.com/p/9fbb9b8ed5fb)<br>
2.[各种编译器警告](http://fuckingclangwarnings.com/#semantic)<br>