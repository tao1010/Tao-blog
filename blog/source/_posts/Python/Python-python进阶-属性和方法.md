---
title: Python-python进阶-属性和方法
toc: true
date: 2018-07-09 16:40:35
tags: python基础
categories: Python
---

一、私有属性和私有方法

<!-- more -->

1.使用场景

	对象的某些方法和属性，只希望对象内部使用,外部不能访问
2.定义方法
	
	在属性名或方法名前 增加 两个下划线 __
		self.__age = age #私有属性
		def __getAge(self): #私有方法
			pass
	在python中没有真正的私有
		解释器对私有属性或方法的处理方式：在名称前加上 _类名 => _类名__名称
		print("访问私有属性:%d" % xiaomei._Women__age)
		xiaomei._Women__secret2()
		
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
	

三、面向对象 - 多态




	



参考资料:      
1.[黑马视频]()     
2.[W3C-Python](https://www.w3cschool.cn/python/)