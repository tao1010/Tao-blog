---
title: Python-python进阶-属性和方法
toc: true
date: 2018-07-10 09:40:35
tags: python基础
categories: Python
---

一、私有属性和私有方法

<!-- more -->

1.使用场景

	对象的某些方法和属性，只希望对象内部使用,外部和子类都不能访问
2.定义方法
	
	在属性名或方法名前 增加 两个下划线 __
		self.__age = age #私有属性
		def __getAge(self): #私有方法
			pass
	在python中没有真正的私有
		解释器对私有属性或方法的处理方式：在名称前加上 _类名 => _类名__名称
		print("访问私有属性:%d" % xiaomei._Women__age)
		xiaomei._Women__secret2()
	子类对象 
		子类对象方法中，不能访问父类的私有属性或方法
		可以通过 父类 的公有方法 间接访问私有属性或方法
		
3.eg

```python
class Women:

    def __init__(self,name):
        self.name = name
        self.common_age = 18
        #定义私有属性
        self.__age = 18
    def secret1(self):
        print("[%s] 的虚拟年龄[%d] "%(self.name,self.common_age))
    # 私有方法
    def __secret2(self):
        print("[%s] 的 真实年龄[%d]" % (self.name, self.__age))

xiaomei = Women("小芳")
print("访问公有属性:%d" % xiaomei.common_age)
xiaomei.secret1()

# 可以通过以下方式访问私有属性或方法：
print("访问私有属性:%d" % xiaomei._Women__age)
xiaomei._Women__secret2() #可以访问私有方法
xiaomei.secret2()#不能访问私有方法

```
二、面向对象 - 继承			
	
	实现代码的复用，提高效率
1.单继承

	子类拥有父类的所有方法和属性
	可重写父类方法
		覆盖父类的方法；
		对父类的方法进行扩展；
		super().父类方法; # 在需要的位置执行父类方法
		#python2.x 可以采用 父类名.方法(self) -- 不推荐
	传递性(子类可继承父类，可继承父类的父类)
	语法：
		class 类名(父类名)	:
			pass
2.eg:

```python
class Animal:
    def eat(self):
        print("吃...")
    def drink(self):
        print("喝...")
    def sleep(self):
        print("睡...")
        
class XiaoTianQuan(Dog):
	def fly(self):
		print("啸天犬可以飞...")

class Dog(Animal):
    def dark(self):
        print("汪汪汪...")
        
class Cat(Animal):
    def catch(self):
        print("抓老鼠...")
    def sleep(self):
        print("白天睡觉...")

dog = Dog()
dog.sleep()
dog.dark()

cat = Cat()
cat.catch()
cat.sleep()
#解析：
	Dog类是Animal类的子类，Animal类是Dog类的父类，Dog类是从 Animal类继承
	Dog类是Animal类的派生类，Animal类是Dog类的基类，Dog类是从 Animal类派生
```
3.多继承

	子类 可以拥有多个父类，具有所有父类的属性和方法
	语法格式:
		class 子类名(父类名1,父类名2,...):
			pass
	注意事项:
		如果父类之间 存在 同名的属性或方法， 应 避免 使用多继承
	MRO(method resolution order) 主要用于在多继承时判断方法、属性的路径调用顺序
		eg:C(B,A)
		在执行C对象方法时： C类中查找 --无—->B类中查找 --无—->A类中查找 --无—->object类(基类)中查找 --无—->程序报错
	兼容性问题(python2.x和python3.x)
		在定义类时，没有父类时，建议统一使用继承object类
		class A(object):
			pass
		在python3.x中没有使用object基类，会默认使用基类
										
```pyhton
class A(object):

    def testA(self):
        print("A")

class B(object):
    def testB(self):
        print("B")

class C(A,B):
    pass

c = C()
c.testA()
c.testB()
```
三、面向对象 - 多态     
1.概述

	定义:不同的子类对象调用相同的父类方法，产生不同的执行结果；
	作用:增加代码的灵活度；
	使用前提：继承、重写父类方法；
2.eg

```python

class Dog:
    def __init__(self,name):
        self.name = name
    def game(self):
        print("自己玩耍...")


class XiaoTianDog(Dog):
    def game(self):
        print("去天上玩耍...")


class Person:
    def __init__(self,name):
        self.name = name
    def game_with_dog(self,dog):
        print("【%s】和 【%s】玩耍" % (dog.name,self.name))
        dog.game()

xiaoming = Person("小明")
dog = Dog("腊肠")
dog1 = XiaoTianDog("啸天犬")
xiaoming.game_with_dog(dog1)
```
四、类	
	
	类的实例:创建出来的对象
	实例化：创建对象的动作
	实例属性：对象的属性
	实例方法：对象的方法
	
	类是一个特殊的对象，类对象在内存中只有一份
	每个对象都有自己独立的内存空间，保存各自不同的属性
	多个对象的方法，在内存中只有一份，在调用方法时，需要把对象的引用传递到方法内部
1.类属性
	
	给类对象中定义属性，用来记录与这个类相关的特征，不会用于记录具体对象的特征
	属性获取 存在一个向上查找机制
	
```python
class Tool(object):
    # 使用赋值语句，定义类属性，记录创建工具对象的总数
    count = 0

    def __init__(self,name):
        self.name = name
        # 针对类属性做一个计数+1
        Tool.count += 1

tool1 = Tool("工具1")
tool2 = Tool("工具2")
print("创建工具的次数:%d" % Tool.count)
```
2.类方法

	针对类对象 定义的方法
	语法格式：
		@classmethod
		def 类方法名(cls):
			pass
	类方法需要使用修饰器@classmethod ;
	类方法的第一个参数应该是cls
	通过 类名.调用类方法，不需要传递cls参数
	在方法内部：
		通过cls. 访问类的属性
		通过cls.调用其他的类方法
```python
class Tool(object):
    # 使用赋值语句，定义类属性，记录创建工具对象的总数
    count = 0

    def __init__(self,name):
        self.name = name
        # 针对类属性做一个计数+1
        Tool.count += 1
    # 类方法
    @classmethod
    def test(cls):
        print("这是一个类方法,工具数量:%s"% cls.count)
 
tool = Tool("工具")
print("创建工具的次数:%d" % Tool.count)
Tool.test()
```
3.静态方法
	
	既不需要 访问实例属性或实例方法
	也不需要 访问类属性或类方法

	语法格式：
	@staticmethod
	def 静态方法():
		pass
	调用方式：
	类名.静态方法
	
```python
class Tool(object):

    @staticmethod
    def use_tool():
        print("这是一个静态方法")
        
Tool.use_tool()
```
4.类方法类属性案例

```python
class Game(object):
    # 定义类属性
    top_score = 0

    # 定义初始化方法和实例属性
    def __init__(self,name):
        self.player_name = name

    # 定义静态方法
    @staticmethod
    def show_help():
        print("显示游戏帮助信息...")

    # 定义类方法
    @classmethod
    def show_top_score(cls):
        print("玩家最高分:%d" % cls.top_score)
        
    # 定义实例方法
    def start_game(self):
        print("【%s】开始游戏..." % self.player_name)

Game.show_help()
Game.show_top_score()

game = Game("油爆爆")
game.start_game()
```
五、单例	  

	设计模式：针对某一特定问题的成熟的解决方案
	单例设计模式: 
		让类创建的对象，在系统中只有唯一的一个实例；
		每一次执行 类名() 返回的对象，内存地址是相同的；
1.__new__方法

	__new__是一个由object 基类提供的内置静态方法，作用:
		在内存中为对象分配空间
		返回对象的引用
	使用 类名() 创建对象时，
		python解释器首先会调用__new__方法为对象分配空间
		python解释器获得对象引用后，将引用作为第一个参数传递给 __init__方法(初始化方法)
	重写__new__方法:
		return super().__new__(cls) #必须返回，否则解释器得不到分配了空间的对象引用，不会调用__init__初始化方法
2.eg

```python
class MusicPlayer(object):

    # 定义类属性
    instance = None   #记录是否分配内存地址
    init_flag = False #记录是否执行过初始化方法

    def __new__(cls, *args, **kwargs):
        # 判断类属性是否为空对象
        if cls.instance is None:
            print("创建对象时，new方法会被自动调用，主要用于分配空间")
            # 必须返回 否则 __init___ 方法获取不到分配的内存空间地址
            cls.instance = super().__new__(cls)
        # 返回类属性保存的对象引用
        return cls.instance

    def __init__(self):

        if self.init_flag is False:
            print("播放器初始化...")
            self.init_flag = True
            
# 验证单例模式的唯一性
player = MusicPlayer()
player1 = MusicPlayer()
print("第一个玩家内存地址:%s\n第二个玩家内存地址:%s"%(player,player1))

```

参考资料:      
1.[黑马视频]()     
2.[W3C-Python](https://www.w3cschool.cn/python/)