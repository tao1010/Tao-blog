---
title: Python-python基础-逻辑
toc: true
date: 2018-06-28 14:47:22
tags: python基础
categories: Python
---

一、分支语句

<!-- more -->

1.if语句
	
	格式：
	if 判断条件:	#if顶格写 注意添加冒号:
		dosomething... # 一个tab键或4个空格
	eg1:
		price = float(input("请输入苹果单价(元/斤):"))
		if price > 5:
		    print("苹果价格太贵了,吃不起")
		    ...
	eg2:
		price = float(input("请输入苹果单价(元/斤):"))
		if price > 5:
		    print("苹果价格太贵了,吃不起")
		    ...
		else:
		    print("价格可以接受")
		    ...
	eg3:
		if 条件1:
			dosomething1
		elif 条件2:
			dosomething2
		else:
			dosomething3
	解析：
		只有 if语句时  if语句和下方缩进的部分 看成一个代码块 -- eg1
		if和else语句 以及下方缩进的部分 看成一个完整的代码块 -- eg2
		if、elif、else以及各自缩进的部分，看成一个完成的代码块 -- eg3
2.运算符
	
	在Python中使用运算符(其他相同，仅如下有区别):
		== 	#等于
	不同类型不能进行比较；
3.逻辑运算
	
	Python中逻辑运算符：
		 and #与
		 or  #或
		 not #非
	逻辑运算符可以把多个条件按照逻辑连接起来，变成更复杂逻辑
	eg:
		# 实例4
		age = int(input("请输入0-120之间的年龄:"))
		if (age > 0) and (age < 120):
		    print("年龄正确")
		else:
		    print("年龄错误!")
		    
		# 实例5
		python_score = 80
		c_score = 90
		if python_score > 60 or c_score > 60:
		    print("考试通过")
		else:
		    print("重考")
		
		# 实例6
		is_employee = True
		if not is_employee:
		    print("不允许入内")
		else:
		    print("请进")
4.if嵌套
	
	# 实例7-火车站安检
	has_ticket = bool(input("请输入是否买票1 - 买票 | 0 - 未买票:"))
	if has_ticket:
	    print("有车票,开始安检...")
	    knife_length = float(input("请输入携带道具长度:"))
	    if knife_length <= 20:
	        print("安检通过")
	    else:
	        print("携带管制刀具,长度:%.2f,安检未通过!" % knife_length)
	else:
	    print("请买票!")
5.实例：石头剪刀布
	
	使用随机数：
	1.导入工具包
		import random #导入随机数模块的工具包
	2.使用随机函数(输入 模块名称 后面输入 . 键盘 tab 可查看所有模块函数)
		random.randint(a,b) #返回[a,b]之间的整数，包含a和b

	# 实例7 - 石头剪刀布
	"""
	    1.控制台输入 石头 - 1 | 剪刀 - 2 | 布 - 3
	    2.电脑 随机 出拳
	    3.比较胜负
	"""
	
	import random
	
	person = int(input("请出拳(石头 - 1 | 剪刀 - 2 | 布 - 3):"))
	# computer = 1 # 假设电脑先出石头
	computer = random.randint(1,3)
	computer_str = "石头"
	if computer == 2:
	    computer_str = "剪刀"
	if computer == 3:
	    computer_str = "布"
	
	if (computer == 1 and person == 3) \
	        or (computer == 2 and person == 1) \
	        or (computer == 3 and person == 2): # 电脑先出石头
	
	    print("您赢了")
	elif computer == person:
	
	    print("平局!")
	else:
	
	    print("您输了!")
	print("电脑出拳: %s" % computer_str)

二、循环语句<br>




	
	
参考资料:<br>
1.[黑马视频]()<br>
