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
	专门用于存储一串信息；
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
3.其他
		
	使用关键字 后不需要 ()
	使用函数、方法 都需要 ()	
	
	
二、元祖

三、字典

四、字符串


五、公共方法


六、高级变量


参考资料:<br>
1.[黑马视频]()<br>
2.[W3C-Python](https://www.w3cschool.cn/python/)