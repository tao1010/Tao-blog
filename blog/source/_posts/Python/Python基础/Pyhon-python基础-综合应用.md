---
title: Pyhon-python基础-综合应用
toc: true
date: 2018-07-05 10:24:39
tags: python基础
categories: Python
---

1.需求

<!-- more -->

	1.程序启动显示名片管理系统的欢迎界面，显示功能菜单
		*****************************
		欢迎使用【名片管理系统】V1.0
		
		1.新建名片
		2.显示全部
		3.查询名片
		
		0.退出系统
		******************************
	2.根据数字选择功能
	3.根据功能选择，执行不同的功能
	4.用户名片需要记录用户的姓名、电话、QQ、邮箱
	5.如果查询到指定名片，用户可以选择修改或者删除名片
	
2.使用知识
	
	变量
	流程控制
	函数
	模块
3.框架搭建
	
	1.准备文件 确定文件名， 保证在需要的位置编写代码
	2.编写框架代码 主运行循环 实现基本的用户输入和判断
	3.使用 pass 关键字 作为占位符 保证程序代码结构正确，不执行任何操作！
	4.无限循环 重复操作直到 break 退出循环
4.模块分析

	card_main.py
		
		import card_tools
		import card_input
		
		字符串判断
		pass
		无限循环
		TODO注释 格式: # TODO(作者/邮件) 需要做的事
	card_tools.py
		定义 显示 欢迎界面 函数
		定义 增、删、查、显示 函数
	card_input.py
		#优化之前完成的功能
		定义输入函数
		
5.数据结构分析

	使用 字典 记录 每张名片的详细信息；
		dic = {
			"name":"",
			"phone":"",
			"qq":"",
			"email":""
		}
	使用 列表 统一记录所有名片字典；
		card_list = []
	
	所有名片相关的操作，都需要使用这个列表，所以应该 定义在程序的顶部；
	最初都是空数据；
6.函数分析

	新增名片：
	    1.提示用户输入名片详细信息
    	2.使用用户输入的信息 建立一个名片字典
    	3.将名片字典添加到列表中
    	4.提示用户添加成功
	显示所有名片：
		1.打印表头
		2.打印分割线
		3.打印名片信息
	查询名片：
		1.提示用户搜索姓名
		2.遍历名片列表，查询搜姓名，并作相关提示或显示
	修改名片：
		1.判断是否修改 
			#字符串是否为空 
			eg1：if not name:
			eg2: if len(name) > 0:
		2.创建存储字典
		3.替换列表中的对应元素
		4.提示
	删除名片：
		直接删除(remove等)
7.运行程序
	
	启动python程序
	方法一: 终端
		pyhton3 xx.py
	方法二: 脚本中加入 #！
		#！ 读 shebang
		位置：
			#! 通常在Unix系统脚本中的第一行开头使用：
		作用：
			指明执行这个脚本文件的解释程序；
		步骤：
			1.使用which查询python解释器所做路径
				which python
			2.修改要运行的主 python 文件(xxx.py) 在第一行增加(必须添加在第一行)
				#! /usr/bin/python #解释器路径
			3.修改主 python文件 的文件权限 增加执行权限
				ls -lh #查看权限信息
				chmod +x xxx.py
			4.执行程序（终端）
				./xxx.py
8.源码<br>	
car_main.py

```python
#! /Library/Frameworks/Python.framework/Versions/3.6/bin/python3
"""
程序入口
每一次启动程序，均通过次文件
"""
import card_tools

while True:

    card_tools.show_menu()

    action = input("请选择操作功能:")
    if action in ["1","2","3"]:
        # 新增名片
        if action == "1":
            # TODO(小明) 新建名片
            card_tools.add_card()
        # 显示全部
        elif action == "2":
            # TODO(小王) 显示全部
            card_tools.show_all()
        else:
        # 查询名片
            # TODO(小李) 查询名片
            card_tools.query_card()
    # 退出系统
    elif action == "0":
        # print("您选择的操作是【退出系统】")
        print("欢迎再次使用【名片管理系统】")
        break
    # 操作异常
    else:
        print("您的操作有误,请选择:【1.新增名片】【2.显示全部】【3.查询名片】【0.退出系统】")
```
card_tools.py

```python
"""
对名片进行 增、删、改、查

"""

fun_count = 30
fun_str = "*"
fun_sub_str = "-"

# 记录所有的名片字典
card_list = []

def show_menu():
    """显示菜单"""
    card_name = "名片管理系统"
    card_v = "V1.0"
    card_new = "1.新建名片"
    card_show_all = "2.显示全部"
    card_query = "3.查询名片"
    card_quit = "0.退出系统"

    print(fun_str * fun_count)
    print("欢迎使用【%s】%s" % (card_name, card_v))
    print("")
    print(card_new)
    print(card_show_all)
    print(card_query)
    print("")
    print(card_quit)
    print(fun_str * fun_count)

def add_card():
    """新增名片"""
    print(fun_sub_str * fun_count)
    print("【新增名片】")
    # 1.提示用户输入名片详细信息
    name = input("请输入姓名:")
    phone = input("请输入电话:")
    qq = input("请输入QQ号码:")
    email = input("请输入邮箱:")

    # 2.使用用户输入的信息 建立一个名片字典
    card_dic = {"name":name,
                "phone":phone,
                "qq":qq,
                "email":email}
    # 3.将名片字典添加到列表中
    card_list.append(card_dic)
    # 4.提示用户添加成功
    print("添加 %s 的名片成功!" % name)

def show_all():
    """显示所有名片"""
    print(fun_sub_str * fun_count)
    print("【显示全部】")
    if len(card_list) > 0:
        # 1.打印表头
        for name in ["姓名", "电话", "QQ", "邮箱"]:
            print(name, end="\t\t")
        print("")
        # 2.打印分割线
        print("=" * fun_count)
        for dict in card_list:
            print("%s\t\t%s\t\t%s\t\t%s" % (dict["name"],dict["phone"],dict["qq"],dict["email"]))
    else:
        print("没有名片记录，请使用添加功能新增名片!")
def query_card():
    """查询名片"""
    print(fun_sub_str * fun_count)
    print("【查询名片】")
    if len(card_list) > 0:
        query_name = input("请输入要查询的姓名:")
        for card_dic in card_list:
            if card_dic["name"] == query_name:
                print("姓名\t\t电话\t\tQQ\t\t邮箱")
                print("=" * fun_count)
                print("%s\t\t%s\t\t%s\t\t%s" % (card_dic["name"], card_dic["phone"], card_dic["qq"], card_dic["email"]))
                print("=" * fun_count)
                editCard(card_dic)
                break
            else:
                print("抱歉,没有找到 %s 名片" % query_name)
    else:
        print("没有名片信息!")

def editCard(dic):
    """修改|删除|返回上一级名片"""
    action = input("请选择要执行的操作(【1.修改】【2.删除】【0.返回上一级菜单】):")

    if action == "1":
        print("【修改名片】")
        # 1.提示用户输入名片详细信息
        # 2.替换列表中的名片
        dic["name"] = input_card_info(dic["name"],"请输入姓名:(回车不修改)")
        dic["phone"] = input_card_info(dic["phone"], "请输入电话:(回车不修改)")
        dic["qq"] = input_card_info(dic["qq"], "请输入QQ号码:(回车不修改)")
        dic["email"] = input_card_info(dic["email"], "请输入邮箱:(回车不修改)")
        # 3.提示用户添加成功
        print("修改 %s 的名片成功!" % dic["name"])
        # show_menu()
    elif action == "2":
        print("【删除名片】")
        card_list.remove(dic)
        print("删除成功!")
    else:
        # show_menu()
        pass
def input_card_info(old_str,place_holder):
    """
    :param old_str: 字典中原有值
    :param place_holder: 输入占位符
    :return: 如果用户输入了内容则返回输入内容，否则返回原有值
    """
    # 1.提示用户输入内容
    new_str = input(place_holder)
    # 2.针对用户输入进行判断，如果用户输入了内容，直接返回结果
    if len(new_str) == 0:
        return  old_str
    # 3.如果用户没有输入内容，返回 字典中原有的值
    else:
        return new_str
```

参考资料:<br>
1.[黑马视频]()<br>
2.[W3C-Python](https://www.w3cschool.cn/python/)