---
title: iOS逆向-工具
date: 2018-05-10 14:50:49
tags: OC
categories: iOS
---

一、Mac工具集		
1.[class-dump](http://stevenygard.com/projects/class-dump/)     

1.1作用:

	用来dump目标对象的class信息的工具；
1.2原理：

	利用oc语言runtime特性，将存储在Mach-O文件中的@interface 和 @protocol信息提取出来，并生成对应的.h文件；
1.3安装：

	将class-dump.dmg文件中的class-dump文件复制到/usr/bin目录(如果OS X 10.11以上，没有/usr/bin文件夹写权限，将class-dump复制搭配/usr/local/bin目录即可)；
	 在终端执行 sudo chmod 777 /usr/bin/class-dump命令赋予其执行权限；
	 运行class-dump；
1.4使用：
	
	接上步骤，运行class-dump；
    将ipa包改后缀名为zip，解压获取.app文件;
    终端执行命令:	class-dump -H xxx/xxx/xx.app -o xx/xx/存放生成的h文件的文件夹；
  		eg: 1.sudo chmod 777 /usr/local/bin/class-dump
  			 2.class-dump
  			 3.class-dump -H /Users/jingxiankeji_tao/Desktop/Payload/Gold.app -o /Users/jingxiankeji_tao/Desktop/hh
1.5补充：
	
	class-dump执行失败(无法得到想要的.h文件或.h文件内容是加密的密文)
		由于class-dump对象必须是未经加密的可执行文件，从appstore上下载的App都是经过签名加密的，可执行文件被加上了一层“壳”，因此需要砸壳(工具AppCrackr)
2.Theos	
2.1简介		

	Theos是一个越狱开发工具包，特点：下载安装简单、Logos语法简单、编译发布简单。
2.2安装和编译				
	
	iOS SDK
		模拟器SDK
	配置环境变量：
		export THEOS=/opt/theos		
		通过此命令设置安装路径，/opt/theos是推荐路径
	获取Theos：
		sudo git clone git://github.com/DHowett/theos.git $THEOS
		或者https://github.com/DHowett/theos 直接下载至$THEOS
	安装ldid	 		
		ldid是专门用来签名iOS文件的工具，用以取代Xcode自带的codesign.
		下载地址:https://github.com/downloads/rpetrich/ldid/ldid.zip
	配置MobileSubstrate环境
		终端:sudo $THEOS/bin/bootstrap.sh substrate
		此时命令无效可用以下解决方案:
			用iFunBox或iTools等软件将iOS设备上的/Library/Frameworks/CydiaSubrate.framework/CydiaSubstrate;
			文件复制到Mac中;
			运行命令：sudo mv -f /path/to/CydiaSubstrate $THEOS/lib/libsubstrate.dylib
	安装dpkg
		dpkg是专门制作deb(Debian package)的工具，用theos开发出来的插件都将以deb格式发布。
		下载地址:http://www.macports.org/install.php
		安装完成，终端命令：sudo port selfupdate
		确保MacPorts升级搭配最新版本，终端命令：sudo port install dpkg
		在iOS的命令行中执行 "dpkg -i xxxdeb" 来安装某个deb文件
			eg：dpkg -i com.hangcom.monitor_0.0.1-40_iphoneos-arm.deb
	安装Theos NIC templates
		下载地址：https://github.com/DHowett/theos-nic-templates/archive/master.zip
		将解压的5个.tar文件复制搭配 $THEOS/templates/iphone下即可
		
2.3使用
	
	cd 到指定目录；
	启动NIC；终端命令:/Users/jingxiankeji_tao/theos/bin/nic.pl
	...
	...
	...

2.4tweak示例



3.Reveal			

3.1简介
	
	Reveal一个UI分析工具，和其他工具(PonyDebugger)比起来更直观。
	界面元素全部以树的形式展现 ，能快速定位到各个UI元素；
	官方的Reveal只能调试自己的APP，对第三方App需要对其做进一步扩展；
3.2安装
	
	下载地址:http://revealapp.com/;
3.3功能扩展
	
	使用方法：
		方法一 Static Library Intergration
		方法二 Dynamic Library Intergration
	
	官网方法：在代码中注入dylib，对逆向工程无用；
		- (void)startReveal{
			
			NSString *revealLibName = @"libReveal.dylib";
			NSArray *paths = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory,NSUerDomainMask,YES);
			NSString *documentsDirectory = [paths objectAtIndex:0];
			NSString *dyLibPath = [NSString stringWithFormat:@"%@%@", documentsDirectory, revealLibName];
			_revealLib = NULL;
			_revealLib = dlopen([dyLibPath cStringUsingEncoding:NSUTF8StringEncoding],RTLD_NOW);
			if(_revealLib == NULL){
				
				char *error = dlerror();
				NSLog(@"dlopen error: %s",error);
			}else{
				
				[[NSNotificationCenter defaultCenter] postNoticficationName:@"IBARevealRequestStart" object:self];
			}
		}
	由于Reveal标准模式不支持查看没有源码的App，因此需要扩展：
		获取libReveal.dylib
			在Reveal中，点击菜单Help-->Show Reveal Library in Finder,获取libReveal.dylib文件；
		将libReveal.dylib导入目标App的目录
			方法一：通过SSH的方式将文件上传；
			方法二：使用iTools工具；
			将libReveal.dylib放在Documents目录下；
		创建RevealUtil
			...
			...
		使用Theos
			...
			...
		安装monitor插件
			...
			...
4.IDA			
4.1简介
	
	IDA是一个支持Windows、Linux、Mac OS X的多平台反汇编/调试器
	下载地址:https://www.hex-rays.com/products/ida/index.shtml
4.2使用
	
4.3分析

5.其他工具
	
5.1iTools
	
	主要用于逆向时的文件传输，常用目录:/var/mobile/Application 和 /var/root
5.2dyld_decache
	
5.3MesaSQLite
	

二、iOS工具集		
1.SBSettings		

2.MobileSubstrate

3.OpenSSH

4.GDB		
4.1简介

4.2使用

5.Cycript

6.其他常用工具		
6.1BigBoss Recommended和Tools
	

6.2AppCrackr

6.3iFile

6.4MobileTerminal

6.5Vi IMproved

6.6SQLite

6.7top

6.8syslogd



参考资料：

1.iOS逆向工程-沙梓社(书籍)		
2.[工具下载:class-dump](http://stevenygard.com/projects/class-dump/)     
3.[class-dump安装和使用](https://www.cnblogs.com/chars/p/5312644.html)    
4.[class-dump使用](https://www.jianshu.com/p/b37b19864fd5)     
5.[工具下载:Theos](https://github.com/DaSens/Theos)      
6.[下载地址:ldid](https://github.com/downloads/rpetrich/ldid/ldid.zip)    
7.[ldid-Saurik](www.saurik.com/id/8) 	
8.[下载地址:dpkg](http://www.macports.org/install.php)     
9.[下载地址:Reveal](http://revealapp.com/)      
10.[下载地址:IDA](https://www.hex-rays.com/products/ida/index.shtml) 		