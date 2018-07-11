---
title: Python-python基础-模块和文件
toc: true
date: 2018-07-11 09:30:02
tags: python基础
categories: Python
---

一、异常

<!-- more -->

1.异常概念

	程序运行时，遇到错误，会停止程序的执行，并提示一些错误信息(抛出异常)
2.处理异常 - 捕获异常
	
	通过 异常捕获 针对突发事件做集中处理，保障程序的稳定和健壮。
	错误类型: python解释器 抛出异常 时,最后一行错误信息的第一个单词就是错误类型。
	语法1:
		try:
			# 尝试执行的代码
		except:
			# 出现错误的处理
	语法2:
		try:
			# 尝试执行的代码
		except 错误类型1:
			# 针对错误类型1,对应的代码处理
		except (错误类型2,错误类型3):
			# 针对错误类型2和3,对应的代码处理
		except Exception as result:
			print("未知错误 %s" % result)
	语法3:
		try:
			# 尝试执行的代码
		except 错误类型1:
			# 针对错误类型1,对应的代码处理
		except (错误类型2,错误类型3):
			# 针对错误类型2和3,对应的代码处理
		except Exception as result:
			print("未知错误 %s" % result)
		else:
			# 没有异常才会执行的代码
		finally:
			# 无论是否有异常，都会执行的代码
3.异常的传递

	当函数或方法 执行出现异常，会将异常传递给函数或方法的调用一方;
	如果传递到主程序,仍然没有异常处理，程序才会被终止
	在开发中，只需在主函数中增加 异常捕获即可
4.抛出raise异常
	
	主动抛出异常场景:
		登录模块时输入的密码不合法时，可主动抛出异常
	python 提供了一个 Exception 异常类
	步骤：
		1.创建一个Exception 的对象；
		2.使用 raise 关键字 抛出异常对象；
5.eg:

```python
# 异常
# 当用户输入非数字时,程序抛出异常
num = input("请输入整数:")
try:
    print("输入的整数为:%d" % int(num))
except:
    print("输入的不是整数")
print("-"*50)
# eg2
try:
    res = 8 / int(input("请输入整数:"))
    print(res)
except ZeroDivisionError:
    print("除 0 错误")
except ValueError:
    print("请输入正确的整数")
except Exception as result:
    print("未知错误 %s" % result)
print("-" * 50)
# eg3: - 主动抛出异常
def input_password():
    # 1.提示用户输入密码
    pwd = input("请输入密码:")
    # 2.判断密码长度 >= 8,返回用户输入的密码
    if len(pwd) >= 8:
        return  pwd
    # 3.密码长度 < 8 主动抛出异常
    # 1> 创建异常对象 - 可以使用错误信息字符串作为参数
    exce = Exception("密码长度不够")
    # 2> 主动抛出异常
    raise exce
    print("主动抛出异常")

# 提示用户输入密码
try:
    print(input_password())
except Exception as result:
    print(result)
```
二、模块 - 扩展    
1.模块的概念

	模块：每一个以扩展名py结尾的Python源代码文件；
	模块名 是一个标志符，需要符合命名规则；
	在模块中定义 全局变量、函数、类 都是提供给外界直接使用的工具；
	模块就好比工具包，要使用必须 导入 这个模块；
2.模块导入
	
	方法一：
		import 模块名1,模块名2
	方法二： 推荐
		import 模块名1
		import 模块名2
	使用工具:
		模块名.工具名(全局变量/函数/类)
	模块别名：符合大驼峰命名法
		import 模块名1 as 模块别名
3.导入部分工具
	
	form ... import
	#从 模块 导入 某一工具
	form 模块名1 import 工具名
		
	使用工具时，不需要指定模块名，直接调用模块提供的工具(全局变量/函数/类)即可
	如果两个模块，存在同名的函数，后导入的模块函数会覆盖先导入的函数
	import 代码 应该统一写在代码顶部，发现冲突可以使用 as 关键字 给其中 一个工具起别名
	eg:
		from hm_01_测试模块1 import say_hello as module1_say_hello
		from hm_02_测试模块2 import say_hello
4.导入所有工具 - 不推荐(函数重名不好处理)

	#从 模块 导入 所有工具
	from ... import *
5.模块搜索顺序

	导入模块时，python解释器搜索顺序：
		
		搜索 当前目录 指定模块名的文件，
		如果有直接导入；
		没有，再搜索 系统目录
	给文件起名时，不要和 系统模块文件 重名
	查看模块的完整路径：__file__ 内置属性
		print(__file__)
		print(random.__file__)
6.导入文件时，文件中 所有没有任何缩进的代码 都会被执行
	
	__name__属性：
		测试模块的代码在 测试情况下被执行，被导入时不会被执行
	
	# 如果直接执行 显示结果为:__main__
	print(__name__)
	
	# 导入模块时，测试代码不会被执行
	if __name__ == "__main__":
	    print(__name__)	
常见Python文件中的代码:

```python
	# 导入模块
	# 定义全局变量
	# 定义类
	# 定义函数
	
	# 在代码的最下方
	def main():
		# ...
		pass
	#根据 __name__判断是否执行下方代码
	if __name__ == "__main__":
		main()
```
7.包 - Package

	包 是一个 包含多个模块 的特殊目录；
	目录下有一个 特殊的文件 __init__.py
	包名的命名方式和变量名一致： 小写字母+_
	
	外界使用包模块，需要在__init__.py中指定 对外界提供的模块列表
	# 从 当前目录 导入 模块列表
	eg:
		"""
		message（文件夹）
			send_message.py
				#定义方法 send
			receive_message.py
				#定义方法 receive
			__init__.py
		导入包.py
		"""
		
		# __init__.py
		from . import send_message
		from . import receive_message
		
		#导入包.py
		import message
		
		hm_message.hm_send_message.send("Hello")
8.发布模块      
![release](release.png)      
步骤:
	
	1.创建 setup.py - 固定格式
		from  distutils.core import setup
		setup(name="message",#包名
		      version="1.0",#版本
		      description="发送接收消息模块",#描述
		      long_description="",#详细描述
		      author="tao",#作者
		      author_email="deng_dev@163.com",#作者邮箱
		      url="www.python.com",#主页
		      py_modules=["receive","send"]#包文件)
	2.构建模块 
		python3 setup.py build
	3.生成发布压缩包
		python3 setup.py	sdist
	最后生成 dist/message-1.0.tar.gz 将这个包发个使用者即可
要制作哪个版本(python2.x或python3.x),就要哪个版本解释器执行    
9.安装和卸载模块:

	# 安装模块
	cd xxx # 收到模块的路径位置
	tar -zxvf message-1.0.tar.gz #解压缩 生成 message-1.0的文件夹(setup.py、PKG-INFO)
	cd xxx #包含setup.py文件的路径位置
	sudo python3 setup.py install

	# 删除导入的模块
	cd xxx # 安装的模块位置路径
	sudo rm -r 模块名*
10.第三方模块的管理
	
	第三方模块 通常指由知名第三方团队 开发的 被广泛使用的 Python包/模块
		pygame # 游戏开发模块
	pip 是通用的python 包管理工具(查找、下载、安装、卸载)
	
	#安装/卸载 模块 到 Python2.x 环境
	sudo pip install pygame
	sudo pip uninstall pygame
	#安装/卸载 模块 到Python3.x 环境
	sudo pip3 install pygame
	sudo pip3 uninstall pygame

	Mac下安装iPython
	sudo pip install ipython
	
	Linux下安装iPython
	sudo apt install ipython
	sudo apt install ipython3
三、文件     
1.文件存储
	
	文件是以 二进制 的方式 保存在磁盘上的
	文本文件
		可以用文本编辑器查看
	二进制文件
		保存的内容不能直接阅读，需要专门软件打开的文件
2.文件的基本操作
	
	步骤：(必须安装步骤执行)
	1.打开文件(文件区分大小写) 			open
		文件存在，返回文件操作对象
		文件不存在，抛出异常
	2.读、写文件内容 	read write
		可以一次性 读入 并返回所有内容
	3.关闭文件 			close
		若忘记关闭文件，会造成系统资源消耗，会影响到后续对文件的访问
		方法执行后，会把文件指针移动到文件的末尾
	read/write/close 需要通过文件对象 来调用
3.eg
	
```python
file = open("file.md")
print(file)

text = file.read()
print(text)

print("-"*50)
# 文件指针 在文末尾，text1 无内容
text1 = file.read()
print(text1)

file.close()
```	
4.文件指针

	标记从哪个位置开始读区数据；
	第一次打开文件时，通常 文件指针 会指向 文件的开始位置；
	当执行 read 方法后，默认 文件指针 会指向 文件的末尾位置；
5.文件打开方式

	默认 只读方式打开
	open("文件名",“打开方式”)
	
	r	默认只读 文件指针在文件的开头 没有文件抛出异常
	w	只写 文件存在覆盖原有内容，不存在文件则创建新文件
	a 	追加 文件存在时，文件指针在文末尾，文件不存在，创建新文件写入
	r+  读写 文件存在时文件指针在文件的开头， 没有文件抛出异常
	w+  读写 文件存在则覆盖原有内容，文件不存在新建文件
	a+  读写 文件存在时文件指针在文件文末尾，文件不存在新建文件写入
	
	频繁的移动文件指针，影响文件读写效率
	
	readline 方法一次性读取一行内容，方法执行完后，文件指针移动到下一行，准备再次读取
```python
print("-"*50)

file = open("file.md")
while True:
    text = file.readline()
    if not text:
        break;
    # 读取 一行的末尾已经有一个'\n'
    print("%s" % text,end="")
file.close()
```
6.文件复制	   
小文件复制
	
	一次性读取，一次性写入
大文件复制
	
	逐行读区，逐行写入
eg

```python
# 复制文件
# """
# 1.打开文件
file_read = open("file.md")
file_write = open("file1.text","w")

# 2.读、写文件
# 小文件复制
# text = file_read.read()
# file_write.write(text)

#大文件复制
while True:
    # 读取一行内容
    text = file_read.readline()

    # 判读是否读取到内容
    if not text:
        break;
    file_write.write(text)

# 3.关闭文件
file_read.close()
file_write.close()

# """
```
7.文件/目录的常用操作
	
	在Python中，导入os模块 即可执行文件/目录的 创建、重命名、删除、改变路径、查看目录内容
	文件操作
		rename  os.rename(源文件名,目标文件名)
		remove  os.remove(文件名)
	目录操作
		listdir 	目录列表		os.listdir(目录名)
		mkdir 		创建目录		os.mkdir(目录名)
		rmdir 		删除目录		os.rmdir(目录名)
		getcwd 	获取当前目录	os.getcwd()
		chdir 		修改工作目录	os.chdir(目标目录)
		path.isdir 	判断是否是文件	os.path.isdir(文件路径)
	文件/目录 都支持相对路径和绝对路径
	os - option system 操作系统
8.编码格式
	
	ASCII编码 (American Standard Code for Information Interchange) 
		计算机 256 个字符
		一个ACSCII在内存中占一个字节的空间
	
	UNICODE编码
		UTF-8
			计算机 使用 1-6个字节 表示一个UTF-8字符
			大多数汉字 使用3个字节 表示
	Python2.x 默认ASCII编码
	Python3.x 默认UTF-8编码
	
	Python2.x 中
		使用中文
			在源文件的第一行 添加(推荐第一种)
			# *-* coding:utf-8 *-* 
			或
			# *_* coding:utf-8 *_*  
			或
			# coding=utf8
		遍历字符串中的字符
			在字符串的“”前加上小写字母 u 即可
			eg: hello = u"hello世界"
四、eval函数

	eval()函数 将字符串 当成有效的表达式来求值，并返回结果
		eval("1+1") 			# 2
		eval("'*' * 3") 		# ***
	不要使用 eval() 函数直接转换input 的结果

参考资料:		
1.[黑马视频]()        
2.[W3C-Python](https://www.w3cschool.cn/python/)    
