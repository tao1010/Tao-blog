---
title: Python-linux基础-系统信息
toc: true
date: 2018-06-25 11:06:13
tags: linux
categories: Python
---

一、系统信息

<!-- more -->

1.系统日期和时间

	date #查看系统时间
	cal  #calendar查看日历，-y选项可以查看一年的日历
2.磁盘空间
	
	df -h  #disk free显示磁盘剩余空间
	du -h[目录名] #disk usage 显示目录下的文件大小
	
	-h选项 表示以人性化方式显示文件大小
3.进程信息 (正在执行的程序)
	
	ps aux 			 #process status 查看进程的详细状况
		a	显示终端上的所有进程，包括其他用户的进程
		u	显示进程的详细状态
		x	显示没有控制终端的进程
	top 				 #动态显示运行中的进程并且排序
		要退出 top命令 输入 q 即可
	kill [-9] 进程代码 #终止指定代号PID的进程，-9表示强行终止 
		最好只终止由当前用户开启的进程，不要终止由root身份开启的进程,否则可能导致系统crash
	eg：kill 4179 
		kill -9 4179
二、其他命令

1.查找文件 - find

	格式:
		find [路径] -name “*.py” #查找指定路径下扩展名是 .py 的文件 包括子目录
	eg:
		find -name "*.png"
		find Desktop/ -name "*1*"
	tip：
		如果省略路径，表示在当前文件夹下查找；
2.文件软链接 - ln	

	格式：
		ln -s 被链接的源文件 链接文件 #建立文件的软链接，类似于快捷方式
	tips：
		没有 -s 选项建立的是一个硬链接文件；
		源文件使用绝对路径，不能使用相对路径，这样移动链接文件后，仍能正常使用；
	eg:
		ln -s test/1.txt 02 #相对路径 不可用
		ln -s /Users/xxx/Desktop/test2/1.txt 01 #绝对路径 可用
		
	在Linux中，文件名和文件数据是分开存储的；
	硬链接：文件的另外一个名字，需要把硬链接所有的文件名删除，才能删除文件数据；
	软链接：文件的绝对路径，从未访问文件的数据；
3.打包压缩 tar

	Windows 常用rar
	Mac 常用zip
	Linux 常用 tar.gz
	
	tar 是Linux中最常用的备份工具，只打包，不压缩；
	格式:
		#打包文件
		tar -cvf 打包文件.tar 被打包的文件/路径...
		
		#解包文件
		tar -xvf 打包文件.tar
	tar选项说明：
		c	生成档案文件，创建打包文件
		x	解开档案文件
		v	列出归档解档的详细过程，显示进度
		f	指定档案文件名称，f后面一定是.tar文件 必须放在选项最后(cxv的顺序可任意调换)
	eg：
		tar -cvf test.tar 1.pdf 2.py 2.txt #在当前目录中将1.pdf、2.py、2.txt三个文件 打包为 test.tar包		tar -xvf test.tar #解包
4.压缩和解压缩 <br>
方法一：

	tar和gzip 命令结合可以实现文件的打包和压缩
	用gzip 压缩tar打包后的文件，其扩展名一般用xxx.tar.gz
	格式：
		压缩文件
		tar -zcvf 打包文件.tar.gz 被压缩的文件/路径...
		
		解压缩文件
		tar -zxvf 打包文件.tar.gz
		
		解压缩到指定路径
		tar -zxvf 打包文件.tar.gz -C 目标路径	选项
		-C 解压缩到指定目录(要解压缩的目录必须存在）
	eg:
		tar -zcvf test1.tar.gz 1.pdf 2.py 2.txt
方法二：

	tar和bzip2 命令结合可以实现文件 打包和压缩
	用bzip2压缩tar打包后的文件，其扩展名一般用xxx.tar.bz2
	格式：
		压缩文件
		tar -jcvf 打包文件.tar.bz2 被压缩的文件/路径...
		
		解压文件
		tar -jxvf 打包文件.tar.bz2
	eg:
		tar -jcvf test.tar.bz2 1.pdf 2.py 2.txt
		tar -jxvf test.tar.bz2
4.安装软件 	apt-get

	apt 是Advanced Packaging Tool是Linux下的一款安装包管理工具；
		 可以在终端方便的 安装、卸载、更新软件包；
	安装软件:
	sudo apt install 软件包
	
	卸载软件：
	sudo apt remove 软件名
	
	更新软件:(更新已安装的包)
	sudo apt upgrade		
5.配置软件源
	
	软件源
	镜像源：所有服务器的内容是相同的，但是根据所在位置不同，国内服务器通常更快
	
参考资料:    
1.[黑马视频 - 密码:m8tu](https://pan.baidu.com/s/1o3eZ1nJTKDi4PRZpeUizgw)<br>
2.[在Mac OS X上开启ssh服务](https://blog.csdn.net/xicikkk/article/details/53447025)<br>