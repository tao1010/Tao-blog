---
title: Python-python进阶-面向对象和封装
toc: true
date: 2018-07-09 10:07:53
tags: python基础
categories: Python
---

1.面向过程和面向对象

<!-- more -->
	
	面向过程：
		实现：
			把完成某个需求的 所有步骤 从头到尾 逐步实现
			根据需求，将某些 功能独立 的代码 封装 成一个一个的函数
			完成代码，顺序调用 不同的函数
		特点:
			注重 过程和步骤，不注重 职责和分工
			需求复杂，代码会很复杂
			开发复杂项目 没有固定套路 难度很大
		相对函数 面向过程没有返回值(函数能执行，可以返回结果)
	面向对象
		三大特征：封装、继承、多态
		实现：
			完成某个需求，首先确定 职责 - 要做的事情
			根据职责 确定不同的对象 在对象内部 封装不要的方法（多个）
			完成代码 顺序地让 不同对象 调用不同的方法
		特点：
			注重 对象和职责 不同的对象承担不同的职责
			适合应对复杂的需求变化 是专门应对复杂项目开发，提供固定套路
			需要在面向过程的基础上，掌握面向对象的语法
		相对函数 面向对象是更大的 封装 根据职责在一个对象中封装多个方法
2.类和对象

	类:对一群具有相同特征或行为的事物的一个统称，是抽象的，不能直接使用，是一个模版，负责创建对象
		特征 --> 属性
		行为 --> 方法
	对象：由类创建出来的具体存在，可以直接使用
		不同对象，属性可能各不相同
	先有 类 再有 对象，类只有一个，对象可以很多
设计类的三个要素:
	
	类名 - 大驼峰命令法 eg:Person
	属性 - 类的特征
	方法 - 类的行为
3.面向对象语法

	在Python中，变量、数据、函数都是对象
	定义类:(第一个参数必须是self，类名必须符合大驼峰命名法)
		class 类名:
			#self:哪一个对象调用的方法，self就是哪一个对象的引用
			def 方法1(self,参数列表):
				pass
			def 方法2(self,参数列表):
				pass
	创建对象:
		对象变量 = 类名()
		#解释器自动执行:
			为对象在内存 分配空间--创建对象；
			为对象属性 设置初始值 - 初始化方法(init)
			__init__ 是对象的内置方法，专门定义一个类具有哪些属性的方法；
	创建属性:
		方法一：不推荐 对创建的对象 采用赋值方法
			对象变量.属性名 = 属性值
			可以通过self. 访问对象的属性
			可以通过self. 调用其他对象的方法
		方法二：在类的内部封装属性
	初始化方法:
		初始化方法(init) 为对象属性 设置初始值
		__init__ 是对象的内置方法，专门定义一个类具有哪些属性的方法；
		方法一:def __init__(self):
	   		 		print("初始化方法")
	   		 		self.name = "Tom"
	   	方法二:def __init__(self,new_name):
	   				print("初始化方法")
	   		 		self.name = new_name	 	
	eg:
		#创建类
		class Cat:
		
	   		 def __init__(self):
	   		 	print("初始化方法")
	   		 	self.name = "Tom"
	   		 
	   		 """
	   		 def __init__(self,new_name):
	   		 	self.name = new_name
	   		 """
	   		 
		    def eat(self):
		        print("cat like eat fish")
		        print("%s like eat fish" % self.name)
		        
		    def run(self):
		        print("cat like run")
		#创建对象     
		tom = Cat()
		"""
		tom = Cat("Tom")
		"""
		
		#创建属性 - 不推荐
			#弊端： 不能调换顺序(python 执行从上而下)，若放在方法后tom.eat()，找不到赋值语句
		tom.name = "Tom" #结合self.name使用 
		
		tom.eat()
		tom.run() 
4.对象内置方法

	__del__ #对象被从内存中销毁前 会被自动调用
	
	__str__ #返回对象的描述信息,print 函数输出使用 
	使用场景:
		在开发中，希望使用print 输出对象变量时，能够打印自定义的内容 就可以利用__str__ 内置方法
		必须返回字符串
5.面向对象 - 封装	 

	对象方法的实现细节 封装在类的内部
	把属性和方法封装在类中
	类与类之间保留2个空行
	一般那个类先被创建，就需要先构建该类
	定义属性，不确定设置的初始值时，可以设置为 None
	一个对象的属性 可以是其他类创建的对象
6.身份运算符(is / is not)

	is / is not 用于比较两个对象的内存地址 是否一致 
	==  用于判断 引用变量的值 是否相等
	Python中针对 None 比较时，建议使用 身份运算符 判断
7.eg:

```python
# 跑步与体重案例
class Person:

    def __init__(self,name,weight):
        self.name = name
        self.weight = weight
    def __str__(self):
        return "我的名字是%s 体重: %.2f公斤" % (self.name,self.weight)
    def __del__(self):# pyhton语句执行完 就会销毁 对应创建的对象
        print("%s 被销毁了" % self.name)
    def run(self):
        self.weight -= 0.5
        print("%s 体重减轻了 0.5kg" % self.name)
    def eat(self):
        self.weight += 0.8
        print("%s 体重增加了 0.8kg" % self.name)
#创建 小明 对象
xiaoming = Person("小明",75.0)
xiaoming.run()
xiaoming.eat()
print(xiaoming)

#创建 小美 对象
xiaomei = Person("小妹",45.3)
xiaomei.eat()
xiaomei.run()
```
```python
# 摆放家具案例
class HouseItem:#先定义
    """创建家具类
     属性:
        名称
        占地面积
    """
    def __init__(self,name,area):
        self.name = name
        self.area = area
    def __str__(self):
        return "[%s] 占地 %.2f" % (self.name,self.area)

    def __del__(self):
        print("家具 %s 被销毁了" % self.name)


class House:
    """
    创建房子类
    属性：
        户型
        面积
        剩余面积
        家具名称列表
    方法:
        添加家具
        """
    def __init__(self, house_type, area):
        self.house_type = house_type
        self.area = area
        # 剩余面积
        self.free_area = area
        # 家具列表
        self.item_list = []

    def __str__(self):
        return ("户型:%s\n总面积:%.2f[剩余面积:%.2f]\n家具:%s"
                % (self.house_type, self.area,
                   self.free_area, self.item_list))

    # 添加家具
    def add_item(self, item):
        print("添加 %s" % item)
        if self.free_area >= item.area:
            self.free_area -= item.area
            self.item_list.append(item.name)
        else:
            print("剩余面积不足,无法添加 %s" % item.name)

# 1.创建家具
bed = HouseItem("席梦思",4.0)
chest = HouseItem("衣柜",2.0)
table = HouseItem("餐桌",1.5)
print(bed)
print(chest)
print(table)

# 2.创建房子
home = House("两室一厅",80.0)

home.add_item(bed)
print(home)

home.add_item(chest)
print(home)

home.add_item(table)
print(home)

```	
```python
# 士兵突击案例
class Gun:
    """
    创建枪类
    属性：
        枪的型号
        子弹数量
    方法：
        装子弹方法 - 增加子弹
        发射子弹
    """
    def __init__(self,model):
        self.model = model
        self.bullet_count = 0

    def add_bullet(self,count):
        self.bullet_count += count

    def shoot(self):
        if self.bullet_count <= 0:
            print("[%s] 没有子弹了!" % self.model)
            return
        # 发射子弹
        self.bullet_count -= 1
        print("[%s] ......🔥剩余子弹:%s"%(self.model,self.bullet_count))

class Soldier:
    """
    创建士兵类
    属性：
        name
        gun
    方法：
        fire
    """
    def __init__(self,name):
        self.name = name
        # 新兵没有枪
        self.gun = None
    def __str__(self):
        return "[%s]的枪是[%s]" % (self.name,self.gun)
    def fire(self):
        # 判断是否有枪
        #if self.gun == None:
        if self.gun is None:
            print("[%s] 没有枪"%self.name)
            return
        # 检查子弹
        if self.gun.bullet_count <= 0:
            self.gun.add_bullet(50)

        print("别动...")
        # 发射
        self.gun.shoot()
# 创建枪对象
ak47 = Gun("AK47")
# ak47.add_bullet(50)
# ak47.shoot()

xusanduo = Soldier("许三多")
xusanduo.gun = ak47
xusanduo.fire()
print(xusanduo)

```

参考资料:      
1.[黑马视频]()     
2.[W3C-Python](https://www.w3cschool.cn/python/)