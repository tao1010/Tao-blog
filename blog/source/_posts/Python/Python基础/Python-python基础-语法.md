---
title: Python-python基础-语法
toc: true
date: 2018-06-27 16:40:25
tags: python基础
categories: Python
---

一、算数运算符

<!-- more -->

	+ - * /  # 加减乘除
	//  # 取整除 eg 9 // 2 输出 4
	**  # 幂 eg： 次方、乘方 2 ** 3 = 8
	
	"-" * 20	#输出连续20个‘-’
先乘除后加减<br>
同级运算符 从左到右<br>
二、程序执行原理<br>
1.计算机核心硬件
	
	CPU
		中央处理器，一块超大规模的集成电路
		负责处理数据/计算
	内存
		临时 存储数据 断点后数据消失
		速度快
		空间小 单价高
	硬盘
		永久存储数据
		速度慢
		空间大 单价低
2.程序执行原理

	程序 运行 之前 是保存在 硬盘 中
	当要运行一个程序时
		操作系统会让 CPU 把程序复制到 内存 中
		CPU 执行 内存 中的程序代码
	程序要执行，首先要被加载到 内存
3.Python程序执行原理
	
	
	操作系统会让 CPU 把 Python解释器 程序复制到 内存 中
	Python解释器 根据语法规则， 从上而下 让 CPU 翻译 Python程序中的代码
	CPU 负责执行翻译完的程序代码
	
	确认解释器的位置
	which python
	
	查看python文件大小
	ls -lh /usr/bin/python
	
	查看具体文件大小
	ls -lh /usr/bin/python
4.程序作用

	程序 用来 处理数据 的,变量 就是用来 存储数据 的
三、变量<br>
1.变量的定义
	
	每个变量在使用前必须赋值,变量赋值后才能被创建
	eg:
		qq_number = 'x123456x'
		qq_password = '123456'
		print('QQ号码: ' + qq_number)
		print('QQ密码: ' + qq_password)
	可以用其他变量的运算值 定义一个变量
	eg:
		price = 5.0
		weight = 3
		total = price * weight
2.变量命名

	标志符就是变量名和函数名；
		字母、数字、下划线；
		不能以数字开头；
		不能与关键字重名；
	关键字：Python内部已经使用的标志符；
		import  #可以导入一个“工具包”
		查看关键字：
			import keyword #导入关键字工具包
			print(keyword.kwlist)	# 输出关键字列表
	Python 中的标志符 是 区分大小写的
	变量名由多个单词组成时： - Python中推荐方法
		每个单词都使用小写;
		单词与单词间使用 下划线(_) 连接;
		eg:first_name、last_name
	驼峰命名法：
		小驼峰
			firstName、lastName
		大驼峰：
			FirstName、LastName
3.变量类型
	
	在Python中，定义变量时不需要指定变量类型
	在运行时，Python解释器，会根据赋值语句等号右侧的数据自动推导出变量中保存的数据的准确类型
	
	数字型
		bool(True 或 False)
		float
		int # python2.x中整型包含int、long
		complex(复数型 - 科学计算，平面场、波动、电感电容等)
	非数字型
		字符串 - str、
		列表、
		元祖、
		字典
	使用 type 函数 查看变量类型
	eg:
		name = "Jike"  #str
		age = 18		  #int
		gender = True  #bool
		type(name)
4.不同类型变量间的计算

	数值型变量可以直接计算
		布尔型 参与数字运算 True - 1  | False - 0
	eg:
		a = True
		b = 10.5
		c = b - a #  9.5	

	字符串变量间 用 + 拼接字符串
	eg:
		firstName = "dong"
		lastName = "haha"
		name = firstName + lastName
		
	字符串变量 和 整数 用 * 拼接
	eg:
		firstName = "dong"		
		name = firstName * 3 # 表示 将3个连续的firstName字符串 赋值给 name
四、变量的输入输出<br>
1.输入
	
	如果要获取用户在键盘上输入的信息，需要使用 input 函数；
	用户输入的任何内容python都认为是一个字符串；
	语法：变量 = input("提示信息")  
	eg:
		input("请输入登录账号:")
		name = input("请输入登录账号:") # 用变量 name 接收 键盘输入的内容
2.变量类型转换
	
	int(x)		 #将 x 转换为一个 整数
	float(x)	 #将 x 转换为一个 浮点数
	str(x)		 #将 x 转换为一个 字符串
	eg:
		type(int("123"))		#将字符串123 --> 整数123
3.eg:
	
	price = input("请输入 apple 单价 （元/斤）：")
	weight = input("请输入 apple 重量 (斤):")
	money = float(price) * float(weight)
	print("顾客支付金额 (元): " + str(money))
	
	等价于
	
	price = float(input("请输入 apple 单价 （元/斤）: "))
	weight = float(input("请输入 apple 重量 (斤): "))
	print("顾客支付金额 (元): " + str(price * weight))
4.格式化输出
	
	%s #字符串
	%d #有符号十进制整数 %06d 表示输出的整数显示位数，不足的地方使用0补齐
	%f #输出 浮点数 %.02f表示小数点后显示2位
	%% #输出%
	格式：
		print("格式化字符串" % 变量1)
		print("格式化字符串" % (变量1,变量2,变量3...))
	eg:
		print("顾客支付金额 (元): %s " % str(price * weight))		print("顾客支付金额 (元): %.2f " % (price * weight))
		print("顾客支付金额 (元): %06d " % int(price * weight))
参考资料:<br>
1.[黑马视频]()
