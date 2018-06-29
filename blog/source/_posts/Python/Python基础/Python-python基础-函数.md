---
title: Python-python基础-函数
toc: true
date: 2018-06-29 13:16:07
tags: python基础
categories: Python
---

一、函数的概念和作用

<!-- more -->

1.函数的概念

	把具有独立功能的代码块 组织成一个小的模块 在需要的时候调用
2.函数的使用
	
	定义函数 -  封装 独立的功能
	调用函数
3.函数的作用
	
	提高编码效率；
	解决代码重用；
二、函数的使用<br>
1.格式
	
	def 函数名():
		函数封装的代码
		...
	解析:
		def  define的缩写
		函数名 见名知意，能表达函数的功能
		函数名	名称规则：
			字母、数字、下划线
			不能一数字开头
			不能和关键字重名
2.使用步骤：<br>
方法一：在不同文件定义、调用函数

	1.新建文件 hm_custom_function.py
	2.编写函数代码
		#函数一： 九九表
		def multiple_table():
		    print("==================九九乘法表==================")
		    row = 1
		    while row <= 9:
		        col = 1
		        while col <= row:
		            if col == row:
		                print("%d * %d = %d" % (col,row,(col * row)))
		            else:
		                print("%d * %d = %d" % (col, row, (col * row)),end="\t")
		            col += 1
		        row +=1
		    print("==================九九乘法表==================")
		#函数二： say hello
		def say_hello():
		    print("Hello Python")
	3.函数的使用
		导入函数文件 - 在需要使用函数的文件中 
			import hm_custom_function
		调用函数
			hm_custom_function.multiple_table()
			hm_custom_function.say_hello()
方法二：在同一文件定义、调用函数
	
	必须 先定义 再调用 函数(Python的解释器必须要先知道函数的存在)
	
	#定义函数
	def say_hello():
		""" 函数说明"""
		print("Hello Python")
	#使用函数
	say_hello()
3.函数的调试<br>
![function](function.png)
	
	单步进入 - step into
	单步越过 - step out
4.函数文档说明
	
	在定义函数的下方输入：
		""" 函数说明 """
	eg:
		def say_hello():
			""" 函数说明"""
			print("Hello Python!")
三、函数的参数<br>
1.函数参数的使用
	
	在函数名后的()内 写入参数
	多个参数之间 使用 , 分隔
2.参数的作用
	
	增加函数的通用性，针对相同的数据处理逻辑，能够适应更多的数据
	形参
		定义函数时，小括号内的参数
		函数内部，可做变量使用
	实参
		调用函数时，传递的参数
3.eg:
	
	#hm_cumstom_function.py
	
	def sum_2_num(num1,num2): #定义函数
   		"""实现两个数字的求和"""
	    print("%d + %d = %d" % (num1,num2,(num2 + num1)))
	
	#hm_04_函数.py
	import hm_custom_function #导入自定义函数
	
	hm_custom_function.sum_2_num(10,20)	#函数调用
四、函数的返回值
1.返回值	

	函数完成执行后，给调用者的一个结果；
	使用 关键字return 可以返回结果；
	使用 return 后，后续代码不会再执行；
	使用 变量 接受 返回值；
五、函数的嵌套

1.tips
	
	函数的调用可以在定义之前
2.eg:

	def test1():
		print("test1")
		test2()
	
	def test2():
		print("*" * 20)
六、模块<br>
1.模块概念

	模块是Python程序架构的核心概念
	类似工具包，想要使用工具包中工具，必须 import 这个模块
		可以使用函数 模块名.函数
		可以使用变量 模块名.变量
	每个以扩展名 .py 结尾的python 源代码文件 都是一个模块
	在模块中定义的 全局变量、 函数 都是模块能够给外界直接使用的工具
2.模块名
	
	是一个标志符；
	命名规则：
		字母、数字、下划线
		不能以数字开头
		不能以下划线开头		
3.其他
	
	pyc文件可提高程序的执行速度
	pyc - 是xx.py 文件通过编译后生成的二进制文件xx.pyc
	若修改了 xx.py文件 编译时会重新生成 xx.pyc二进制文件
参考资料:<br>
1.[黑马视频]()<br>
2.[W3C-Python](https://www.w3cschool.cn/python/)