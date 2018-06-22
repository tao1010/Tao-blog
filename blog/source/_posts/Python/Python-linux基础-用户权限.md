---
title: Python-linux基础-用户权限
toc: true
date: 2018-06-21 14:22:11
tags: linux
categories: Python
---

一、SSH高级<br>

<!-- more -->

1.终端中查看 .ssh 目录<br>
![ssh](ssh3.png)	
	有关SSH配置信息都保存在用户家目录下的 .ssh 目录下
	
	ls-alh #查看隐藏目录
	cd .ssh #进入 .ssh 目录中
	ls -alh #查看目录文件
		已知主机文件 known_hosts
2.远程连接<br>
![ssh](ssh.png)	

	在Mac电脑的 系统便好设置 中，点击 共享 后，勾选 远程登录 即可启用SSH服务
	可通过如下命令远程登录：
		ssh xxx@192.168.xx.xxx
		yes/no 	#是否授权
		password	#输入需要远程服务器密码
	终端命令:exit #退出远程登录
3.免密登录<br>
步骤：
	
	1.配置公钥
		ssh-keygen	#生成 公钥和私钥 输入文件名:xxx 和对应公钥的密码:***，即可生成：xxx.pub 的公钥文件和 xxx的私钥文件 可以直接回车，不输入任何信息，默认生成id_rsa.pub 的公钥文件和id_rsa的私钥文件
	2.上传公钥到服务器
		ssh-copy-id -p port user@remote #让服务器记住公钥,会将xxx.pub文件保存在远程服务器家目录下的 .ssh 目录下	
	完成上述步骤后，在此登录远程服务器时不需要输入密码。		cat xxx.pub #此命令可以查看公钥信息
4.免密登录原理<br>
![ssh2](ssh2.png)<br>

	公钥：xxx.pub  默认生成 id_rsa.pub
	私钥：xxx		 默认生成 id_rsa
	
	非对称加密算法：
		使用 公钥 加密的数据，需要使用 私钥 解密；
		使用 私钥 加密的数据，需要使用 公钥 解密
	
	使用过程:	
	客户端-->服务器
		客户端将 数据A 通过 私钥 加密后，上传服务器，
		服务器接收 数据A 后，使用 公钥 解密;
	服务器-->客户端
		服务器将 数据B 通过 公钥 加密后，下发到客户端，
		客户端接收 数据B 后，使用 私钥 解密;
5.配置别名<br>

	优点：
		ssh -p port user@remote <===> ssh xxx(别名)
	步骤:
		1.在家目录下的.ssh目录下新建config文件；
			cd .ssh
			touch config
			gedit config #貌似无效
		2.编辑config文件内容:
			Host xxx(别名)
				HostName ip地址 #远程服务器的ip地址
				User 用户名 #远程计算机的用户名
				Port 22
		3.保存文件，即可使用别名远程登录或scp复制文件等。
			ssh xxx #即可远程登录
			exit	#退出远程登录
			scp -r ~/Desktop xxx:Desktop/Demo #将本地桌面的所有文件 复制到 远程计算机 桌面上 Demo文件夹下
二、用户权限<br>

	在Linux中，每个系统(本机或远程登录系统)必须拥有一个账号，并对于不同系统资源拥有不同的使用权限；
	在Linux中，可以指定每一个用户针对不同的文件或者 目录的不同权限；
	在实际运用中，预先对组设置好权限，然后将不同的用户添加到对于的组中，而不用依次为每个用户设置权限；
	对文件或目录的权限：
		r	read		4(数字代码)		读
		w	write		2				写
		x	excute		1				执行
1.ls -l命令扩展(查看文件详细信息列表)<br>
![lsl](lsl.jpg)<br >
![lsl2](lsl2.png)<br>
	
	硬链接数： 表示有多少种方式访问到当前的文件或目录
<font color='red'>2.chmod - 修改权限</font>
	
	chmod 可以修改用户/组 对文件/目录的权限
	格式:
		chmod +/-rwx 文件名/目录名 #+表示增加权限，-表示减少权限
	
	该方式会一次性修改 拥有者和组权限 
		
	执行文件格式：（文件当前目录下执行）
		./文件名
3.root - 超级用户
	
	Linux系统中，root账户通常用于系统的维护和管理，对操作系统的所有资源具有所有访问权限；
	大多数版本的Linux中，不推荐 直接使用root账号登录系统；
	在Linux安装过程中，系统会自动创建一个标准用户账号
	
	sudo
		su substitute user 的缩写 表示使用另一个用户身份；
		sudo 命令用来以其他身份执行命令，预设身份 root；
		用户使用sudo，必须先输入密码，之后5分钟有效；
		未授权用户使用sudo，则会发警告邮件给管理员；
三、组管理<br>
1.创建和删除组

	对组操作的终端命令需要通过sudo执行
	组信息保存在 /etc/group文件中
	/etc 目录是专门用来保存系统配置信息的目录
	
	添加组：
		groupadd 组名
		eg:sudo groupadd dev
	删除组：
		groupdel 组名
		eg:sudo groupdel dev
	确认组信息:
		cat /etc/group
	修改文件/目录的所属组：
		chgrp 组名 文件/目录名
	递归修改文件/目录的所属组：
		chgrp -R 组名 文件/目录名 
四、用户管理
	
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
3.usermod
	
	设置主组和附加组
	
		
		
	指定用户登录shell
	
	

	
五、系统信息

六、其他命令

七、打包压缩

八、软件安装




参考资料:    
1.[黑马视频 - 密码:m8tu](https://pan.baidu.com/s/1o3eZ1nJTKDi4PRZpeUizgw)<br>
2.[在Mac OS X上开启ssh服务](https://blog.csdn.net/xicikkk/article/details/53447025)<br>