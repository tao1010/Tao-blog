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
	
	合并字典:字典1.update(字典2)
		被合并的字典包含已经存在的键值对，会覆盖原有的键值对
		eg:
			temp_dict = {"weight":75}
			xiaoming.update(temp_dict)
	清空字典:清空所有键值对
		xiaoming.clear()
4.循环遍历

	#变量k是每一次循环中，获取到的键值对的key
	for k in xiaoming:
    	print("字典中的键值对为 key: %s, value:%s" % (k,xiaoming[k]))
5.应用场景

	使用对歌键值对 ，存储描述一个物体的相关信息 - 描述更复杂的数据信息；
	将 多个字典 放在一个 列表 中，进行遍历，在循环体内部针对每个字典进行 相同的处理；
四、字符串<br>
1.字符串的定义
	
	可以一对 “”,也可用一对 ‘’,推荐 “ ”
2.常用方法
	
	获取长度: len(字符串1)
	统计小字符串2 在 大字符串2 中出现的次数: 字符串1.count(字符串2)
	获取指定位置的字符: 字符串1[index]
	获取指定字符在字符串的位置: 字符串1.index(字符串2)
		若 字符串2 不存在 程序报错
	循环遍历:
		for char in 字符串1:
			print(char)
3.方法分类<br>
![判断类型](str-is.png)<br>
![文本查找和替换](str-find.png)<br>
![大小写转换](str-change.png)<br>
![文本对齐](str-just.png)<br>
![去除空白字符](str-strip.png)<br>
![拆分连接](str-split.png)<br>

	判断类型 - 9
	查找和替换 - 7
	大小写转换 - 5
	文本对齐 - 3
	去除空白字符 - 3
	拆分和连接 - 5
4.字符串切片
	
	切片：使用索引来限定范围，从一个大的字符串中切出小的 字符串
	格式:
		字符串[开始索引:结束索引:步长]
	tips:
		不包括结束索引的元素
		顺序索引 0 1 2...
		倒序索引(从右到左) -1 -2 -3 ... 
	eg: 
		num = '0123456789'
		print(num[2:6]) #2345
		print(num[2:])  #23456789
		print(num[:3])	#012
		print(num[0:3])	#012
		print(num[0::2]) #02468
		print(num[2:-1]) #2345678
		print(num[-1::-1]) #9876543210 倒序
		print(num[::-1])	  #9876543210 倒序
五、公共方法<br>
1.定义

	公共方法:列表、元祖、字典、字符串等共有的方法;

2.Python常用函数

	len(item) #元素个数
	del(item)	#删除变量
		a = [1,2,3,4]
		del a[0]#删除列表中下标为0的元素
		print(a)
		del(a[0])#删除列表中下标为0的元素
		print(a)
		del(a) #删除a列表
	max(item) #返回容器中元素的最大值 如果是字典，比较key,返回key
	min(item)	#返回容器中元素的最小值 如果是字典，比较key,返回key
		t_dic = {"a":"1","b":"2","c":"3"}
		print(max(t_dic))
		print(min(t_dic))

	cmp(item1,item2) #Python3.x取消的此函数 比较大小 -1 < | 0 = | 1 >
		Python3.x中可以用比较运算符比较
3.切片
	
	使用对象：字符串、元祖、列表
		"1234"[1:3] 		#字符串
		[1,2,3,4][1:3]	#列表
		(1,2,3,4)[1:3]	#元祖
	字典不能切片 - 键值对形式存在，没有索引
4.运算符<br>
![commonmethod](commonmethod.png)

	t_list = [1,2,3,4]
	
	# .extend 方法会将括号内的列表元素分别插入列表中
	t_list.extend([8,9]) #[1,2,3,4,8,9]
	
	# .append方法会将括号内的对象作为一个元素插入
	t_list.append([8,9]) #[1,2,3,4,[8,9]]
	t_list.append(0)		 #[1,2,3,4,0]
	
	in 和 not in 在判断字典时，判断的是字典的key
	成员运算符(in 和 not in) 用于测试序列中是否包含指定的成员 
5.完整的for循环
	
	for 变量 in 集合:
		...
	else: #上面的for 循环集合中的每个元素都遍历了，就会执行else代码
		没有通过break 退出循环，循环结束后，会执行的代码
	
	eg：
		for num in [1,2,3,4]:
		    print(num)
		    if num == 2:
		    	print("不执行else代码")
		    	break;
		else:
		    print("循环结束!")
参考资料:<br>
1.[黑马视频]()<br>
2.[W3C-Python](https://www.w3cschool.cn/python/)