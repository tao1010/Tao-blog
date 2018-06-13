---
title: iOS逆向-砸壳
date: 2018-06-13 09:06:09
tags: 逆向
categories: iOS
---

前提：
	
	iOS设备(iPhone 或者 iPad 均可)已越狱
一、工具和软件		
1.[PP助手Mac版](https://pro.25pp.com/pp_mac_ios)<br>
2.iOS微信App(正式) 6.6.7 - Mac PP助手下载<br>
3.[dumpcrypted - 砸壳](https://github.com/stefanesser/dumpdecrypted)<br>
4.[class-dump - 导出头文件](https://github.com/nygard/class-dump)<br>
5.[iOSOpenDev1.6-2 - 替换方法](http://www.iosopendev.com)<br>
6.[insert_dylib - 动态注入方法](https://github.com/Tyilo/insert_dylib)<br>
   [yololib - 动态注入](https://github.com/KJCracks/yololib)<br>
7.[MachOView - 查看注入是否成功](https://sourceforge.net/projects/machoview/)<br>
8.[iOS App Signer - 签名工具](https://github.com/DanTheMan827/ios-app-signer)

二、资源和环境		
1.macOS Hign Sierra 10.13.4<br>
2.iOS11.2.1 和 iOS11.3.1 <br>
3.Xcode9.3.1<br>
三、操作步骤<br>
1.class-dump导出头文件
	
	class-dump -H XXX/Payload/WeChat.app -o XXX/WeChatHeaders 

2.查看 WeChatHeaders 文件夹 
	
	若文件夹中只有 CDStructures.h 文件,且文件中什么也没有,则需要砸壳;
	若文件夹中包含11695个文件，则完成头文件的导出步骤;
3.未导出文件继续执行下述操作。[已导出文件-传送门](https://tao1010.github.io/2018/06/08/iOS/iOS逆向/iOS逆向-实战-微信红包和步数/)<br>
4.在已越狱的iOS设备上安装软件

	通过Cydia安装
		OpenSSH - 实现远程登录
		Cycript - 在命令行下实现与应用的交互
	PP助手
5.确保iOS设备和Mac处于同一局域网中，在Mac终端输入命令：

	ssh root@192.168.1.121 远程登录已越狱的iOS设备;
		默认密码: alpine
		iOS设备局域网IP地址:192.168.1.121
6.在越狱的iOS设备上运行微信App后，Mac终端输入命令：

	ps -e | grep WeChat
	解析:
		此命令用于查找WeChat可执行文件的路径;
	记录此可执行文件的路径A
7.通过Cycipt查找WeChat的Document路径，Mac终端输入命令:
	
	cycript -p WeChat
	解析：
		此命令用于进入cycript命令状态
8.在cycript命令状态下，输入命令:

	NSSearchPathForDirectoriesInDomains(NSDocumentDirectory,NSUserDomainMask,YES)[0]
	解析:
		获取WeChat的Document路径
	记录此结果后的路径B
9.将砸壳工具dumpdecrypted 拷贝到WeChat的Document目录下用于砸壳
	
	将命令切回Mac OS X
	输入: scp dumpdecrypted.dylib root@192.168.1.121:路径B(Document路径)
10.重新远程登录到iPhone，使用dumpdecrypted.dylib砸壳，具体用法：

	DYLD_INSERT_LIBRARIES=/路径B(Documents路径)/dumpdecrypted.dylib 路径A(可执行文件路径)
11.出现下图表示砸壳成功<br>
![complete](complete.png)<br>
在当前路径下生成 WeChat.decrypted 文件<br>
![success](success.png)<br>
12.将iOS设备上生成的 WeChat.decrypted 文件拷贝到Mac上

	scp WeChat.decrypted mac登录用户名@192.168.1.121:XXX/Desktop/
	解析:
		jingxiankeji_tao - mac登录用户名 
		存放将要拷贝的 WeChat.decrypted 文件路径 - XXX/Desktop/
13.检查 WeChat.decrypted 文件 是否砸壳成功<br>
![check](check.png)<br>		

	第一个cryptid 0表示armv7架构已成功，
	第二个cryptid 1表示arm64未成功
	理论上只要把最老的架构解密就可以了，因为新的cpu会兼容老的架构；所以这里arm64未成功不影响	
14.再次远程连接iOS设备，拷贝WeChat.app待用

	ssh root@192.168.1.121 远程登录已越狱的iOS设备;
		默认密码: alpine
		iOS设备局域网IP地址:192.168.1.121
	scp -r 路径A(可执行文件路径的上一级 WeChat.app)@192.168.1.60:XXX/Desktop/
	解析：
		路径A - iOS设备上存放 WeChat.app 的路径
		XXX/Desktop/ - 存放将要拷贝 WeChat.app 的路径
15.砸壳完成,[导出 .h 文件-传送门](https://tao1010.github.io/2018/06/08/iOS/iOS逆向/iOS逆向-实战-微信红包和步数/)<br>

参考资料：			
1.[非越狱微信步数和红包](https://www.jianshu.com/p/7c0c2bcbbaf2)	<br>
2.[dumpdecrypted - 砸壳](https://www.jianshu.com/p/a4373b5feca0)<br>
3.[iOSRE逆向](http://bbs.iosre.com)<br>
4.[iOS微信自动抢红包-入门教程](https://www.jianshu.com/p/ad578bef4b76)<br>
