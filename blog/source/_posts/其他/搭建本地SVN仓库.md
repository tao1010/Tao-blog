---
title: 搭建本地SVN仓库
date: 2018-06-13 17:10:15
tags: SVN
categories: 其他
---

1.Mac自带SVN
	
	查看版本信息:
		svnserve --version
2.创建代码仓库
	
	sudo mkdir -p ~/DeskTop/SVNStore
3.初始化代码仓库
	
	sudo svnadmin create ~/Desktop/SVNStore 
![svnstore](svnstore.png)<br>

4.配置SVN权限
	
	修改SVNStore --> conf 文件夹下内容:

	svnserve.conf 配置用户权限
		anon-access = read
		auth-access = write
		
		password-db = passwd
		
		authz-db = authz
		解析：
			代表匿名访问的时候是只读的，若改为anon-access = none代表禁止匿名访问，需要帐号密码才能访问
			删除前面的注视，不要留空格
	passwd 配置账号信息
		[users]
		deng = 123456
		deng1 = 123456
		解析：
			在[users]下添加用户(eg:用户名=密码)
	authz 配置权限
		[groups]
		iosdev = deng,deng1
		...
		[/]
		@iosdev = rw
		
		解析：
			配置名为 iosdev 的用户组,组下用户为 deng，deng1(如果多个用户，用‘,’分割)
			在最下面添加 [/] 表示授权目录路径访问权限, @ioddev = rw 表示给 iosdev 组 读写权限(r - 读,w - 写,rw - 读写)
			如果只允许用户访问项目下Demo文件目录,则:
				[/Demo]
				@iosdev = rw
			@iosdev 表示授权给iosdev组,不使用 @ 则表示授权给用户
5.启动svn服务
	
	svnserve -d -r ~/Desktop/SVNStore 
	解析:
		回直接启动配置好的 SVNStore SVN服务器 默认使用80端口
		如不使用默认的端口:
			svnserve -d -r ~/Desktop/SVNStore --listen-port 8080
6.使用Cornerstone等工具连接SVN<br>
![dengSVN](dengSVN.jpg)<br>
	
	在Cornerstone中的Server： 输入为本地电脑IP地址
7.添加内容，提交<br>
![commiterror](commiterror.png)<br>
	
	1.终端: sudo chmod -R a+w ~/Desktop/SVNStore
	2.终端: 输入电脑管理员密码
	
		即可成功提交!

参考资料:<br>
1.[Mac上搭建本地SVN仓库](https://www.jianshu.com/p/0c06f80a2561)
