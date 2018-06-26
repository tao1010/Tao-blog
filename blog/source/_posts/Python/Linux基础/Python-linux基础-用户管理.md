---
title: Python-linux基础-用户管理
toc: true
date: 2018-06-25 10:02:42
tags: linux
categories: Python
---

一、用户管理

<!-- more -->

	用户管理包括用户和组的管理
1.创建用户/设置密码/删除用户
	
	终端命令都需要使用sudo执行
	增加新用户：
		useradd -m -g 组 新建用户名 
		# -m自动建立用户家目录 
		# -g指定用户所在的组，否则会建立一个和同名的组
		eg: sudo useradd -m -g dev deng #表示在dev群组下创建一个deng的新用户
	设置密码：
		passwd 用户名 #普通用户直接使用passwd可以修改自己密码
		eg:sudo passwd deng
	删除用户：
		userdel -r 用户名 #-r选项会自动删除用户家目录
		eg:sudo userdel -r deng
	确认用户信息：
		cat /etc/passwd|grep 用户名 #新建用户后，用户信息会保存在 /etc/passwd 文件中
		eg：cat -n /etc/passwd #-n表示显示行号的家目录信息列表
			cat -n /etc/passwd | grep deng #显示deng的用户信息
	passwd文件
		/etc/passwd 文件存放的是用户的信息，由6个分号组成的7个信息：
			1.用户名
			2.密码(x,表示加密的密码)
			3.UID(用户标识)
			4.GID(组标识)
			5.用户全名或本地账号
			6.家目录
			7.登录使用的Shell，即登录之后使用的终端命令，ubutu默认dash
2.查看用户信息
	
	查看用户UID和GID信息：
		id [用户名] #G group组
	查看当前所有登录的用户列表：
		who 
	查看当前登录用户的账户名：
		whoami
3.usermod<br>
设置主组和附加组

	主组：通常在新建用户组时指定，在/etc/passwd 的第4列GID对应的组；
	附加组：在etc/group 中最后一列表示该组的用户列表，用于指定用户的附加权限
		
	修改用户的主组（g）
	usermod -g 组 用户名
	
	修改用户的附加组（G）
	usermod -G 组 用户名
		
指定用户登录shell(shell 即为终端)
	
	修改用户登录shell
	usermod -s /bin/bash

tips
	
	设置了附加组后，需要重新登录才能生效；
	默认使用 useradd 添加的用户没有权限使用 sudo，以root身份执行命令的，可以使用以下命令将用户添加到sudo 附加组中
		usermod -G sudo 用户名
4.which
	
	用于查看执行命令所在的位置
	eg:
		which ls
		which useradd
	
	/etc/passwd 		 用于保存用户信息的文本文件
	/usr/bin/passwd	 用于修改用户密码的程序
	验证: 查看对应文件权限 
		ls -l /usr/bin/passwd
		ls -l /etc/passwd

bin和sbin
	
	在Linux中，绝大多数可执行文件都是保存在 /bin、/sbin、/usr/bin、/usr/sbin
	/bin (binary)是二进制执行文件目录，主要用于具体应用
	/sbin	(system binary)是系统管理员专用的二进制代码存放目录，主要用于系统管理
	/usr/bin (user commands for applications)后期安装的一些软件
	/usr/sbin (super user commands for applications)超级用户的一些管理程序
5.切换用户 su

	格式：
		su -用户名   #切换用户，并且切换目录 ， - 的作用：可以切换到用户家目录，否则保持位置不变
		exit 退出当前登录账户
	su 不接用户可以切换到root，但是不推荐使用，因为不安全
![exit](exit.png)<br>	
6.修改文件权限

	chown 修改文件/目录的拥有者
	chgrp 修改文件/目录的组
	chmod 修改文件/目录的权限
	格式：
	chown 用户名 文件名|目录名
	chgrp -R 组名 文件名|目录名#-R递归修改文件|目录组
	chmod -R 755 文件名|目录名
			7 文件或目录拥有者的权限 
			5 文件或目录组成员的权限 
			5 文件或目录其他组成员的权限 
chmod设置权限时，可以简单的使用三个数字分别对应 拥有者 / 组 / 其他 用户的权限<br>
chmod +/-rwx 文件名|目录名 #直接修改文件|目录的 读|写|执行 权限。但不能精确到拥有者 / 组 / 其他<br>
![auth](auth.png)<br>
		
	eg:	
		777 u=rwx, g=rwx, o=rwx
		755 u=rwx, g=rx,  o=rx
		644 u=rw,  g=r,   o=r
		000 u=-, 	 g=-,   o=-
	
	eg1:chmod -R 777 01.py
		
参考资料:    
1.[黑马视频 - 密码:m8tu](https://pan.baidu.com/s/1o3eZ1nJTKDi4PRZpeUizgw)<br>
2.[在Mac OS X上开启ssh服务](https://blog.csdn.net/xicikkk/article/details/53447025)<br>