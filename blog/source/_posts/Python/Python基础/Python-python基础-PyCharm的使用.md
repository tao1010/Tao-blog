---
title: Python-python基础-PyCharm的使用
toc: true
date: 2018-06-26 11:53:53
tags: python基础
categories: Python
---

一、PyCharm简介

	版本号：2016.2.3
	
<!-- more -->

1.特点

	是Python的一款非常优秀的集成开发环境；
	可以在Windows、Linux、macOS 下使用；
	适合开发大型项目；
		一个项目通常包含很多源文件；
		每个源文件的代码行数有限，通常子啊几百行之内；
		每个源文件各司其职，共同完成复杂的业务功能；
2.构成<br>
![pycharm1](pycharm1.png)<br>
二、PyCharm设置<br>
1.打开目录
	
	打开一个项目后，会在目录先新建一个 .idea 的目录，用于保存项目相关信息:
		解释器版本
		项目包含文件等
![idea](idea.png)<br>
2.设置项目中运用的 python 解释器版本
	
	路径:菜单栏
	PyCharm --> Preferences -->Project... --> Project Interpreter -->选择解释器版本
	
	运行配置设置：
	Run/Debug Configurations --> Python interpreter --> 选择解释器对应的版本
3.设置字体(控制台和编辑区)
	
	PyCharm --> Preferences --> Editor --> Colors & Fonts
	
三、新建项目<br>
1.命名规则
	
	建议只使用小写字母、数字、下划线
	不能以数字开始
2.新建项目
	
	菜单栏：File --> New Project...
3.注释
	
	单行注释 # 
	解释器不会解释 # 号右侧的代码
	# 放在代码行首 建议在 # 后添加一个空格 或者 command + / 添加注释
	# 放在代码行尾 建议在 # 空格前空2格
	
	多行注视或块注释
	"""  这是一个多行注释 """

参考资料:<br>
1.[黑马视频]()