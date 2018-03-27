---
title: iOS逆向-简介
date: 2018-03-18 16:42:02
tags: iOS逆向
categories: iOS

---

1.作用：

	1）增加新的功能模块
	2）分析已有的功能模块
		a)修复Bug；
		b)修改定制；
		c)学习提高；
	3）换个角度看待自己曾经熟悉的事物；（竞品分析）

2.逆向工程常规套路：

	1）观察、猜测、寻找分析切入点；
	2）用dumpdecrypted给App砸壳；- 解密
	3）用class-dump导出Objective-C头文件；
	4）用Cycript定位目标视图；
	5）获取目标视图的UIViewCOntroller或delegate或model；
	6）在controller或delegate中寻找目标文件的蛛丝马迹；
	7）用hopper和lldb的组合还原调用逻辑；- 汇编
	8）...




参考资料：

1.iOS逆向工程-沙梓社(书籍)