---
title: Python-python基础-第一个程序
toc: true
date: 2018-06-26 10:27:11
tags: python基础
categories: Python
---

1.基本概念

<!-- more -->
	
	Python程序文件扩展名 通常是 .py；
	Pyhton源程序是一个特殊格式的文本文件，可用任意文本编辑软件做Python的开发；
2.创建python程序<br>
方法一：终端
	
	打开终端工具,在桌面创建文件夹python demo
		cd /Users/xxx/Desktop #切换到桌面
		mkdir python demo	  #创建文件夹
	创建python1.py文件
		cd python demo
		touch python1.py 	#创建python1文件
		vim python1.py	   #开始编辑文件
		i						#切换到编辑模式
		#编辑python程序代码
			print("Hello python")
			print("Hello world")
		################
		esc						#退出编辑模式
		:wq						#保存并退出python1.py文件
	执行python1.py文件
		python python1.py	#执行python1.py文件
	此时可在终端看到输出的内容
3.常见错误
	
	语法错误 SyntaxError
		拼写错误
		一行代码多件事情的处理的情况 --- 每行代码执行一个动作
	python2.x默认不支持中文
	缩进错误 IndentationError
		每行代码顶格输入
4.python2.x和3.x区别

	python2.x 的解释器名称 python - 不支持中文
	python3.x 的解释器名称 python3 - 支持中文
		只是在2.0的基础上修改了底层实现代码，所以2.x-->3.x工作量不大
	建议使用3.0的语法，设计2.0的时候不考虑向下兼容
	
	如果开发时，无法立即使用Pyhon3.0，建议使用Python3.0开发，然后使用Python2.6、2.7执行，并且做一些兼容处理。
5.执行Python程序的三种方式<br>
解释器运行Python程序
	
	Python解释器
		python xxx.py	#使用python 2.x 解释器
		python3 xxx.py #使用python 3.x 解释器
	
	其他解释器
		CPython 官方版本的C语言实现
		Jython 可以运行在JAVA平台
		IronPython 可以运行在.NET和Mono平台
		PyPy Python实现，支持JIT即时编译
交互式运行Python程序	

	步骤：
		终端输入:python #进入Python2.x Shell环境
		输入命令代码：
			print("Hello world") # 回车
		退出：
			方式一: exit()
			方式二: control + d
	特点：
		直接在终端中运行解释器，不输入要执行的文件名；
		在python的Shell中直接输入Python代码，会立即看到程序执行的结果；
	适用：
		学习和验证python语法或局部代码
	缺点：
		代码不能保存；
		不适合运行太大的程序；
		
	IPython
	
		IPython(interactive python)是一个python的交互式shell
		特点：
			支持自动补全；
			自动缩进；
			支持bash shell命令；
			内置了许多有用的功能和函数；
		版本：
			ipyhton #python2.x
			ipyhton3 # python3.x
		退出：
			control + d 或者 exit
Python的IDE - PyCharm
	
	集成开发环境(IDE,intergrated development environment)
		图形用户界面；
		代码编辑器；(支持 代码补全，自动缩进)
		编辑器/解释器；
		调试器；(断点/单步执行)
		...
	PyCharm特点:
		可以在Windows、Linux、macOS 下使用；
		适合开发大型项目；
参考资料:<br>
1.[黑马视频]()