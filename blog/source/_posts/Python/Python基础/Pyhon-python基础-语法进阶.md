---
title: Pyhon-python基础-语法进阶
toc: true
date: 2018-07-05 16:38:32
tags: python基础
categories: Python
---

一、变量的引用

<!-- more -->

1.引用的概念

	变量和数据 都保存在内存中，分开存储的;
		数据 保存在内存中的一个位置；
		变量 中保存着数据在内存中的 地址；
	变量 中记录数据的地址 就叫引用；
	使用 id()函数 可以查看变量中保存数据所在的内存地址；
	在python中函数的参数传递以及返回值 都是靠引用传递,返回的值并不是数据而是引用(地址)
	
2.如果变量已经被定义，当给一个变量赋值时，本质上修改了数据的引用(地址)

	变量不再 对之前的数据引用
	变量改为 对新赋值的数据引用
二、可变和不可变<br>
1.不可变
	
	内存中的数据不允许被修改；
	数字类型 int bool float complex long(2.x)
	字符串str
	元祖tuple
2.可变

	内存中数据可以被修改；
	列表list
	字典dict
		key 只能使用不可变类型数据(字符串、整数、元祖，不可使用字典和列表作为key)
		
	可变类型的数据变化(是通过方法来实现的)地址不会变化
	对可变类型的变量 赋值 地址会变化
3.哈希(hash) 	
3.1.pyhton中内置一个名叫 hash(o)的函数

	接收一个  不可变的数据 作为参数；
		若为可变参数(list、dict)程序报错
	返回结果是一个 整数；
	eg：
		hash("hello")	
		hash(1)
		hash((1,))
		
3.2.哈希 是一种算法 
	
	作用：提取数据的特征码(类似于指纹);
	相同的内容得到 相同的结果
	不同的内容得到 不同的结果
3.3.在python中，设置字典的键值对时，首先会对 key 进行hash 决定如何在内存中保存字典的数据，以便后续对字典的操作：增、删、改、查

	键值对 的key 必须是不可变类型数据
	键值对 的value 可以是任意类型数据
三、局部变量和全局变量 		
1.使用范围

	局部变量:函数内部定义的变量，只能在函数内部使用；
	全局变量:函数外部定义的变量，所有函数 内部 都可使用这个变量；
	不推荐 全局变量；
2.局部变量

	不能在 函数内部以外的地方 使用； 
	函数执行结束后，局部变量会被系统回收；
	生命周期：
		被创建(函数执行后)-->被系统回收(函数结束后)
	作用:临时存储函数内部使用的数据；
	不同函数内的局部变量可以相同；		
3.全局变量

	在函数内部：
		可以获取全部变量的数据；
		不允许修改全局变量的值；
		如果使用赋值语句，会在函数内部定义一个局部变量；
	全局变量:可变范围太大，程序不好维护
	
	在函数内部修改全局变量的值：
		
		使用 关键字 gloabl 修饰即可
		eg: global num 
			 num = 100
	全局变量定义的位置:
		模块中的所有全局变量 定义在所有函数上方，保证所有函数都能访问到
	命名:
		建议 - 全局变量名 前缀 g_ 或 gl_
4.代码结构示意图

	#！  shebang
	import 模块
	全局变量
	函数定义
	执行代码
四、函数参数和返回值 	
1.利用元祖返回多个值
	
	元祖包含多个数据，可以使用元祖一次返回多个值；
	如果返回类型是元祖，可以省略小括号
	eg:
		return (temp,wetness)
		return temp, wetness
	接收返回值：
		g_temp,g_wet =  return temp,wetness
2.交换两个数字 - 算法
	
	要求：
		a = 6, b = 100 交换a,b两数字
	方法一：临时变量
		c = b
		b = a
		a = c
		
	方法二：不使用其他变量
		a = a + b
		b = a - b
		a = a - b
	
	方法三：python，元祖
		a,b = b,a #把元祖的小括号省略了
3.可变参数和不可变参数
	
	函数内部针对参数 使用赋值 不会修改外部的实参变量；
	无论可变或不可变 参数，只要针对参数使用赋值语句 会在 函数内部 修改 局部变量 的引用 不会影响到外部变量的引用
4.使用方法修改参数		
	
	在 函数内部 使用方法 修改 可变参数 会改变 外部 实参
	eg：
		def demo(num_list):
			#方法修改参数内容
			num_list.append(9)
			print(num_list)
		g_list = [1,2,3]
		demo(g_list)
		print(g_list)
5.+=运算符 - 算法

	#列表变量 作 += 元算 不会相加再赋值的操作 实际为 extend 方法操作 因此会改变外部参数
	eg:
		num_list = num_list + num_list  # 不会改变外部参数
		num_list += num_list  # 本质操作为 num_list.extend(num_list) 会改变外部参数
6.缺省参数
	
	缺省参数：
		定义函数时，可以给 某个参数 指定一个 默认值，具有默认值的参数
	作用:
		函数的缺省参数，将常见的值设置为参数的缺省值 简化函数的调用
	eg:
		g_list = [6,3,9]
		g_list.sort() #默认按照升序排序
		g_list.sort(reverse=True) #降序排序	
	指定函数的缺省参数：
		最常见方法 - 在参数后使用赋值语句
		eg:
			def get_info(name,gender=True):
				gender_text = "男生"
				if not gender:
					gender_text = "女生"
				print("%s 是 %s" % (name,gender_text))
	定义位置：
		必须保证 带有默认值的缺省参数 在参数列表的末尾
	多个缺省参数的函数：
		def get_info(name,title="",gender=True):
				pass
		get_info("小明",title="hello",gender=False)
7.多值参数
	
	使用场景：
		需要 一个函数 能够处理的参数 个数 是不确定的，需要使用多值参数
	定义方式：
		参数名前 增加一个 * 可以接收 元祖
		参数名前 增加两个 ** 可以接收 字典
	一般多值参数的命名方式 习惯使用2个名字：
		*args     存放 元祖 参数 前面放一个 * #args--arguments
		**kwargs	存放 字典 参数 前面有两个 * #kwargs --keyword arguments
	eg:
		
		def demo(num, *args, **kwargs):
		    print(num)
		    print(args)
		    print(kwargs)
		
		demo(1,2,3,4,5,name="小明",age=18,gender=True)
		输出:
			1
			(2,3,4,5)
			{'name': '小明', 'age': 18, 'gender': True}
元祖和字典的拆包

	拆包 使用场景
		将一个 元祖变量 传递给 args
		将一个 字典变量 传递给 kwargs
	拆包 使用方式：
		在 元祖变量 前 增加一个 *
		在 字典变量 前 增加两个 **
	eg:
		def demo(*args, **kwargs):
			    print(args)
			    print(kwargs)
		
		gl_nums = (1,2,3)
		gl_dic = {'name': '小明', 'age': 18, 'gender': True}
		demo(*gl_nums,**gl_dic)
8.递归	
	
	定义：
		一个函数内部自己调用自己
	特点：
		函数内部代码 相同，针对参数不同，处理结果不同；
		递归出口:当参数满足一个条件时，函数不再执行(否则出现死循环)
	
	eg:
		def test_demo(num):
		    if num == 1:
		        return 1
		    temp = test_demo(num - 1)
		    return num + temp
		print("%s" % test_demo(10))
参考资料:      
1.[黑马视频]()     
2.[W3C-Python](https://www.w3cschool.cn/python/)