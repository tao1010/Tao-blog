---
title: Python-linux基础-环境配置
date: 2018-04-13 17:14:29
tags: linux
categories: Python
toc: true

---

一、Python运行环境

linux环境		
虚拟机VMare Fusion		
[ubuntu](http://www.ubuntu.org.cn/download/desktop)    


二、操作系统（Operation system，OS）    
![03-01](03-01.png)  
1.操作系统的分类：
	
	桌面操作系统
		Windows - 用户多
		MacOS - 适合开发人员
		Linux - 应用软件少 适用于服务器|嵌入式
	服务器操作系统 - 远程管理
		Linux
			安全、稳定、免费
			占有率高
			配套软件齐全
				Python
		Windows Server
			付费
			占有率低
	嵌入式操作系统 
		Linux
			Python
	移动设备操作系统
		iOS
		Android(基于Linux)
2.操作系统的发展		
3.Linux内核版和发行版本		
内核：
	
	是系统的心脏；
	是运行程序和管理像磁盘和打印机等硬件设备的核心程序，提供了一个裸设备与应用程序间的抽象层
发行版：
	
	通常包含桌面环境、包公套件、媒体播放器、数据库等应用软件；
	可搜索"十大Linux服务器发行版"
	Ubuntu
	Redhat
	...
4.Linux文件和目录			

	1.单用户操作系统和多用户操作系统		
	2.Windows和Linux文件系统的区别		
5.Ubuntu系统目录结构演示和简介			
6.Ubuntu图形界面基本使用		
	
	1.Ubuntu任务栏
	2.窗口操作按钮
	3.菜单条
三、虚拟机		
1.虚拟机:通过软件(VMWare)模拟具有完整硬件系统功能的、运行在一个完全隔离环境中的完整计算机系统.	
四、Linux命令			
1.常用的Linux命令的基本使用		

|序号|命令|对应英文|作用|
|---|---|---|---|
|1|ls|list|查看当前文件夹下的内容|
|2|pwd|print wrok directory|查看当前所在文件夹|
|3|cd [目录名]|change directory|切换文件夹|
|4|touch [文件名]|touch|如果文件不存在，新建文件|
|5|mkdir [目录名]|make directory|创建目录|
|6|rm [文件名]| remove|删除指定的文件名|
|7|clear|clear|清屏|

2.终端窗口字体
	
	command + shift + = 放大字体
	command + - 减小字体
3.终端命令格式			
command [-options] [parameter]		
	
	command:命令名，相应功能的英文单词或单词的缩写；
	[-options]:选项，可用来对命令进行控制，也可以省略；
	[parameter]:传给命令的参数，可以时零个，一个或者多个；
	[]: 代表可选
	eg: 
		ls 
		pwd
		clear 
		cd /Users/jingxiankeji_tao/Desktop/前端
		touch 新的文件.txt
		mkdir 新的文件夹
		rm 要删除的文件.txt
注意：命令和参数之间必须要有空格，选项和参数之间必须要有空格.         
4.查询帮助命令信息		
command --help		
	
	显示command命令的帮助信息
man command

	查阅command命令的使用手册;
	q - 退出,空格键 - 显示手册的下一屏,b - 回滚一屏,f - 前滚一屏 ;
	man是manual的缩写;
五、文件和目录命令 		



参考资料:    
1.[黑马视频](https://pan.baidu.com/s/1o3eZ1nJTKDi4PRZpeUizgw)  密码:m8tu
