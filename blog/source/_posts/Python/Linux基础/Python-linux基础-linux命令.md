---
title: Python-linux基础-linux命令
toc: true
date: 2018-06-19 10:01:42
tags: linux
categories: Python
---

一、Linux命令		

<!-- more -->
	
1.常用的Linux命令的基本使用		

|序号|命令|对应英文|作用|
|---|---|---|---|
|1|ls|list|查看当前文件夹下的内容|
|2|pwd|print wrok directory|查看当前所在文件夹|
|3|cd [目录名]|change directory|切换文件夹|
|4|touch [文件名]|touch|如果文件不存在，新建文件|
|5|mkdir [目录名]|make directory|创建目录|
|6|rm [文件名]| remove|删除指定的文件名|
|7|clear|clear|清屏|

2.终端窗口字体
	
	command + shift + = 放大字体
	command + - 减小字体
3.终端命令格式			
command [-options] [parameter]		
	
	command:命令名，相应功能的英文单词或单词的缩写；
	[-options]:选项，可用来对命令进行控制，也可以省略；
	[parameter]:传给命令的参数，可以时零个，一个或者多个；
	[]: 代表可选
	eg: 
		ls 
		pwd
		clear 
		cd /Users/jingxiankeji_tao/Desktop/前端
		touch 新的文件.txt
		mkdir 新的文件夹
		rm 要删除的文件.txt
注意：命令和参数之间必须要有空格，选项和参数之间必须要有空格.         
4.查询帮助命令信息		
command --help		
	
	显示command命令的帮助信息
man command

	查阅command命令的使用手册;
	q - 退出,空格键 - 显示手册的下一屏,b - 回滚一屏,f - 前滚一屏 ;
	man是manual的缩写;
二、文件和目录命令 		
1.Linux下文件和目录的特点		
ls命令扩展：

	以.为起始的文件，默认为隐藏文件,需要用 -a参数能显示所有内容
	. 代表当前目录
	.. 代表上一级目录
|命令|描述|eg|
|---|---|---|
|ls -a|显示指定目录下所有子目录与文件，包括隐藏文件||
|ls -l|以列表方式显示文件的详细信息||
|ls -l -h|配合ls -l以人性化的方式显示文件大小(B,K,M表示大小)||
|ls通配符的使用||
| * |代表任意个数个字符|ls 1* 或ls -*1.txt或ls - *3 *|
|？|代表任意一个字符| ls 1?3.txt|
|[]|表示可以匹配字符组中的任意一个|  |
|[abc]|匹配a,b,c中任意一个|ls -[12345]23.txt|
|[a-f]|匹配a到f中的任意一个|ls -[1-5]23.txt|
多个命令可以连在一起使用，无先后顺序:
	
	eg:ls -lh
		ls -lha / ls -lah
2.自动补全
	
	输入文件名/文件夹名/路径名的头几个字母，按 tab 键自动补全；
	按2下 tab 键 可以筛选出相同起始字母的内容；
3.历史命令切换
	
	上/下 键
	control + c 退出选择
4.命令扩展

|命令|描述|eg|
|---|---|---|
|ls|list，查看目录内容||
|cd|切换目录 change directory||
|touch|创建文件 - 文件不存在；修改文件最后日期-文件已存在| touch 123.txt|
|rm|删除 直接从磁盘删除，不可恢复| rm 123.txt|
|rm -f|强制删除，忽略不存在的文件，无需提示| rm 123.txt|
|rm -r|递归的删除目录下的内容，删除文件夹时必须加此参数| rm -r 新建文件夹|
|rm -r|不能在根目录做此操作| rm -r * 删除当前目录的所有目录和文件|
|mkdir|创建一个新目录(文件夹) 不能与当前目录中已存在的目录同名| mkdir 新建文件夹|
|mkdir -p|可以递归创建目录 不能与当前目录中已存在的目录同名| mkdir -p 父文件夹/子文件夹/A/B/|
|tree [目录名]|tree|以树状图列出文件目录结构|
|cp 源文件 目标文件|拷贝 copy||
|mv|移动||
|cat|||
|more|||
|grep|||
|echo|||

5.切换目录 cd (change directory)
	
	Linux所有的目录和文件名大小写敏感
	cd  切换到当前用户的主目录(home/用户目录)
	cd ~ 切换当当前用户主目录(home/用户目录)
	cd . 保持当前目录不变
	cd .. 切换到上级目录
	cd - 可以在最近两次工作目录之间来回切换
	
	相对路径: 最前面不是/或者～，表示 相对当前目录 所在的目录位置
	绝对路径:最前面是/或者～，表示从 根目录/家目录 开始的具体目录位置
6.拷贝和移动文件
 
|命令格式|英文|作用|例子|
|---|---|---|---|
|tree [目录名]|tree|以树状图列出文件目录结构|tree -d 只显示目录|
|cp 源文件 目标文件|copy|复制文件或者目录|cp ~/Documents/readme.txt ./readme/txt|
|mv 源文件 目标文件|move|移动文件或者目录/文件或者目录重命名||

eg:
	
	cp 1.png 2.png	//复制源文件1.png 到当前目录下，重命名为2.png
	cp 1.png 新建文件夹/2.png //复制源文件1.png 到当前目录下的新建文件夹下，重命名为2.png
	cp -i 1.png 1.png //覆盖文件前操作提示
	cp -r test1 test2 //复制目录test1 到当前目录下 重命名为test2
	
	mv 新建文件夹/1.png 3.png //移动 新建文件夹 下的 1.png文件 到当前目录下 重命名为3.png
	mv -i 1.png test/2.png // 覆盖文件操作提示 移动1.png 到test文件夹下 重命名为2.png
tips：<font color='red'>在终端中 对文件的操作不能回撤</font><br>
7.查看文件内容

|命令格式|英文|作用|例子|
|---|---|---|---|
|cat 文件名|concatenate|查看文件内容、创建文件、文件合并、追加文件内容等功能，适合内容较少的情况||
|more 文件名|more|分屏显示文件内容,每次只显示一页内容，适合内容较多的情况||
|grep 文件名|grep|搜索文本文件内容||
	
more的操作：
	
	空格键 显示手册页的下一屏
	enter 一次滚动手册页的一行
	b 回滚一屏
	f 前滚一屏
	q 退出
	/word 搜索word字符串
grep模式查找:
	
	模式查找又称为正则表达式查找
	
	^a 行首 搜索以a开头的行
	a$ 行为 搜索以a结束的行
	
eg:
	
	cat -b test.txt //  显示test.txt 文件内容 对非空输出行编号
	cat -n test.txt //  显示test.txt 文件内容 对输出的所有行编号
	more test.txt // 显示test.txt 文件内容
	
	grep sa test.txt //搜索test.txt 文件中 包含sa文本
	grep -n sa test.txt // 显示行号  搜索test.txt 文件中 包含sa文本
	grep -v sa test.txt //显示不包含匹配文本 搜索test.txt 文件中 包含sa文本
	grep -vn sa test.txt //显示不包含sa文本的 行号
	grep -i sa test.txt //忽略大小写

	grep ^6 test.txt //搜索以 6 开始的行
	grep n$ test.txt //搜索以 n 结束的行
	
三、其他命令<br>
1.echo

	格式：echo 文本内容
	作用：echo会在终端显示参数指定的文字，通常会和重定向联合使用
2.重定向 > 和 >>

	linux 允许将命令执行结果 重定向到一个文件
	将本应显示在终端上的内容 输出/追加 到指定文件中
	> 表示输出 会覆盖文件原有内容
	>> 表示追加 会将内容追加到已有文件的末尾
3.管道 |
	
	Linux允许将一个命令的输出 可以通过管道 作为另一个命令的输入
	
4.eg:
	
	echo Hello > 11.txt // 在当前目录下 输出 Hello 到11.txt 中 (没有11.txt 则新建11.txt文件)
	echo Hello Python >> 11.txt // 追加 文本 Hello Python 到11.txt文件中的末伟
四、远程管理命令<br>
1.关机和重启<br>

	安全关闭或重新启动系统
	格式:shutdown 选项 时间 
	eg:
		shutdown //默认表示1分钟后关闭电脑
		shutdown -r now //现在重新启动电脑
		shutdown 20:25 //系统在今天20:25关机
		shutdown -c 取消关机或重启命令
		shutdown +10 10分钟之后关机
	tips: 远程维护服务器最好不要关闭系统，而应该重新启动系统
2.查看或配置网卡信息<br>
命令格式：

	ifconfig  #configure a networking interface 查看/配置计算机当前的网卡配置信息
	ping ip地址  #ping 检查到目标IP地址的连接是否正常 ip地址也可以用域名代替(ping www.baidu.com)
	control + c 结束ping
网卡

	一个专门负责网络通讯的硬件设备；
	IP地址是设置在网卡上的地址信息；
	eg: 电脑<==>电话 网卡<==>SIM卡 IP地址<==>电话号码
	tips:一台计算机中可能会有多个物理网卡和多个虚拟网卡，在linux中的物理网卡的名字通常以ensXX表示
iP地址

	每台联网的电脑上都有一个IP地址，保证电脑间正常通讯的重要设备；
	每台电脑的IP地址不能相同，否则会出现IP地址冲突，并且没有办法正常通讯
eg
	
	ifconfig #查看网卡配置信息
	ifconfig | grep inet #查看网卡对应的IP地址 （通过管道输入 过滤查找inet关键字信息）
	
	ping IP地址 #检测到目标主机的连接是否正常
	ping 192.168.122.xx
![ping](ping.png)

	解析:发送56字节的数据到 IP(192.168.122.xx),在time时间内收到对应的64字节的数据回执，时间越短网速越快.
<font color='red'>3.远程登录和复制文件</font><br>
命令格式:
	
	ssh 用户名@ip   #secure shell 关机/重新启动
	scp 用户名@ip:文件名或路径 用户名@ip:文件名或路径 #secure copy 远程复制文件
<font color='red'>ssh(secure shell)基础 - 重点</font>
	
	SSH客户端是一种使用Secure Shell(SSH)协议连接到远程计算机的软件程序；
	SSH是目前比较可靠，专为远程登录会话和其他网络服务提供安全性的协议；
	SSH传输的数据可以经过 压缩 ，从而加快传输速度
		利用SSH协议可以有效防止远程管理过程中的信息泄露；
		通过SSH协议可以对所有传输数据进行 加密 ，防止DNS欺骗和IP欺骗；
	
	域名
		用一串用点分割的名字组成 eg：www.baidu.com
		是IP地址的别名，方便用户记忆
		
	端口号
		IP地址：通过 IP地址 找到网络上的计算机
		端口号：通过 端口号 可以找到计算机上运行的应用程序
			SSH服务器的默认端口号是 22，如果是默认端口号，在连接的时候可以忽略
		常见服务器端口号列表：
			SSH服务器 	默认端口号22
			Web服务器 	默认端口号80
			HTTPS 		默认端口号443
			FTP服务器 	默认端口号21
SSH客户端的简单使用
	
	命令:ssh [-p port] user@remote
	解析：
		user 是在远程机器上的用户名，如果不指定的话默认当前用户
		remote 是远程机器的地址，可以是IP/域名。或者是别名
		port 是SSH Server监听的端口，如果不指定，默认22
	tips: 
		exit #退出当前用户
		ssh终端命令只能在Linux或UNIX系统下使用
		windows系统中，安装Putty或是XShell 客户端软件
		在工作中，SSH服务器的端口号可能不是22，此时需要使用-p选项，指定正确的端口号，否则无法正常连接服务器
	sudo #超级用户的权限	
<font color='red'>scp - 掌握 </font>

	scp就是secure copy 是一个在Linux下用来进行 远程拷贝文件 的命令
	它的地址格式与ssh基本相同，在指定端口时用的是大写的 -P，而不是小写的
	-r 选项表示传送文件夹
	-P 若远程SSH 服务器的端口不是22，需要使用大写字母-P选项指定端口
	eg:
		#把本地当前目录下的 01.py文件 复制到远程 家目录下的 Desktop/01.py
		scp -P port 01.py user@remote: Desktop/01.py
		
		#把远程 家目录下的Desktop/01.py 文件 复制到 本地当前目录下的 01.py
		scp -P port user@remote:Desktop/01.py 01.py
		
		
		#把当前目录下的demo文件夹 复制到远程 家目录下的 Desktop
		scp -r demo user@remote:Desktop
		
		#把远程家目录下的 Desktop 复制到 当前目录下的demo文件夹 
		scp -r user@remote:Desktop demo
4.FileZilla在Windows下文件传输

	使用Filezilla 通过FTP进行文件传输
	端口号 21
	
参考资料:    
1.[黑马视频](https://pan.baidu.com/s/1o3eZ1nJTKDi4PRZpeUizgw)  密码:m8tu<br>
2.[Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)<br>
3.[XShell](http://www.xshellcn.com/)<br>
4.[FileZilla在Windows下文件传输](https://filezilla-project.org/download.php?type=client)

