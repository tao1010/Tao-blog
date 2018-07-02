---
title: Python-python基础-高级变量
toc: true
date: 2018-06-29 15:19:03
tags: python基础
categories: Python
---

非数字型 数据类型 的特点：
	
	都是一个序列，可以看成容器；
	取值 []
	遍历 for in
	计算长度、最大/最小值、比较、删除
	链接 + 和 重复 *
	切片
一、列表 - List	

<!-- more -->

1.定义

	列表(List) 是Python中使用最频繁的数据类型，其他语言称为数组；
	专门用于存储一串信息,存储相同类型的数据；
	列表用 [] 定义，数据之间用 , 分隔；
	列表的 索引|下标  从 0 开始
	eg:
		name_list = ["张三","李四","王五"]
2.列表常用方法<br>
![list_method](list_method.png)	

	xx.extend # 将列表A添加到列表B中时，A、B的数据类型必须一致
	del 列表名[下标] #关键字 del 本质上是用来将变量从内存中删除
	xx.remove['a'] #删除一个存在的元素，如果有多个元素 a 则只删除第一次出现的a
	eg:
		del name_list[1] #将name_list中下标为1的元素 “李四” 从内存中删除
		name_list.count['a'] #统计列表name_list中 ‘a’ 出现的次数 
3.其他
		
	使用关键字 后不需要 ()
		del name_list[1] #删除 name_list 列表下标为1的 数据 内存也删除
	使用函数、方法 都需要 ()	
		name_list.reverse() #反转
		name_list.append['Jack'] #在列表name_list 最后加上 ‘Jack’ 数据
	
	函数和方法的区别：
		方法和函数一样都是封装的独立的功能；
		方法需要通过对象来调用，表示针对这个 对象 要做的操作：
			对象.方法名(参数)
4.循环遍历

	在Python中，为提高列表的遍历效率 专门提供 迭代iteration 遍历
	for循环迭代遍历格式
	eg:
		for name in name_list:
			print(name)
二、元祖 - Tuple<br>
1.定义
	
	和列表类似，区别：
		可以保存不同类型的数据；
		元祖的元素不能修改；
		定义方式为:元祖名()；
	eg:
		tuple = ('张三', 18, 1.75)
		print("姓名:%s \t 年龄:%d \t 身高:%.2f" % (tuple[0],tuple[1],tuple[2]))
2.注意
	
	元祖定义一个元素时，后面也必须加 ,
	eg：info_tuple = (50,)
	解析： 若不加 , python解释器会认会 info_tuple = (50) 这是一个变量名
3.常用方法
	
	eg:tuple = ('张三', 18, 1.75)
	取数据: tuple[1]
	取索引: tuple.index("张三")
	统计计数:	tuple.count("张三")
	包含元素个数: len(tuple)
4.循环遍历
	
	#使用格式字符串频接 元祖 tuple 不方便,因为元祖中保存的数据类型不同
	for item in tulpe:
		print(item)
5，应用场景

	函数的参数和返回值，一个函数可以接受多个参数，或者一次返回多个数据；
	格式字符串	后面的()本质上是一个元祖；
	列表不可以被修改，以保护数据安全；
5.元祖和列表的转换
	
	元祖-->列表
	list(tuple) #可修改tuple数据
	
	列表-->元祖
	tulpe（list）
	eg：
		info_tuple = ('张三', 18, 1.75)
		num_list = [1,2,3,4]
		#列表转元祖
		num_tulpe = tuple(num_list)
		print(num_tulpe)
		#元祖转列表
		info_list = list(info_tuple)
		print(info_list)
三、字典 - dictionary<br>
1.定义
	
	字典用 {} 定义,可用来定义多个数据；
		通常用于存储描述一个 对象 的相关信息
	使用 键值对 存储数据，键值对间使用 , 分隔;
	键和值间用 : 分隔;
	键必须唯一的同时只能使用字符串、数字、或元祖，值可以是任何数据类型；
	eg:
		xiaoming = {"name":"小明",
					  "age" :18,
					  "gender": True,		
					  "height": 1.75}
2.和列表的区别：
	
	字典：是 无序 对象的集合
	列表: 是 有序 对象的集合
3.字典的方法

	获取所有 key 列表: 字典.keys()
	获取所有 value 列表: 字典.values()
	获取所有 (key,value)元祖 列表: 字典.items()
	获取键值对数量: len(字典)
	
	查：xiaoming["name"] #key错误或不存在 程序报错
	增：xiaoming["weight"] = 75 #key不存在 新增
	改：xiaoming["age"] = 20	#key存在 修改
	删：xiaoming.pop("gender") #删除 指定key 的键值对,若key错误或不存在，程序报错
	
	
	
四、字符串


五、公共方法


六、高级变量


参考资料:<br>
1.[黑马视频]()<br>
2.[W3C-Python](https://www.w3cschool.cn/python/)