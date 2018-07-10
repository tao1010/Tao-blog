---
title: iOS库-静态库开发
toc: true
date: 2017-03-21 15:19:06
tags: iOS第三方库
categories: iOS
---

Cocoa Touch Static Library    

	将方法的具体实现对使用者屏蔽,仅使用.a静态库 和 .h 文件 引入项目中
	适用场景：.h文件比较少的情况下可以使用生成静态库的方法
步骤       

<!-- more -->

1.创建静态库项目   
![creat](creat.png)
![file](file.png)    
2.开发静态库对应的功能
	
	可删除xxx.m文件
3.生成静态库
	
	真机静态库
		在Xcode上选择 Generic iOS Device 
	模拟器静态库
		在Xcode上选择 任意一个模拟器 	
	编译项目(成功:在Products文件夹下的红色.a文件会变成黑色.a文件)后，在Products文件夹 选中.a文件 Show in Finder即可找到生成的.a文件
	
4.合并真机和模拟器的.a文件

	终端输入:
	lipo -create 模拟器.a文件路径 真机.a文件路径 -output 输入路径文件夹/文件名.a
	查看.a库支持的架构：
	lipo -info 模拟器.a文件路径 # 模拟器架构 i386
	lipo -info 真机.a文件路径 # 真机架构 armv7、armv7s、armv64
	
4.汇总相关文件

	新建一个 ‘xxxSDK’的文件夹，将步骤4生成的.a文件拖入其中，同时将项目中所有的.h文件也一并拖入其中
5.使用
	
	将 ‘xxxSDK’ 的文件导入需要的工程项目中,在对应位置导入头文件,根据.h文件内容即可调用对应方法;
参考资料:     
1.[制作.a静态库](https://blog.csdn.net/addychen/article/details/46637483)     
2.[SDK开发-.a静态库](https://www.jianshu.com/p/65b1c1326c50)