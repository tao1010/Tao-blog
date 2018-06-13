---
title: iOS逆向-简介
date: 2018-03-18 16:42:02
tags: 逆向
categories: iOS

---
一、逆向工程简介		
1.作用：
		
- 攻破目标程序，获取关键信息 - 安全相关的逆向工程；

		评定安全等级；
		逆向恶意软件；
		检查软件后门；
		去除软件使用限制；
- 借鉴他人程序功能，开发自己软件 - 开发相关的逆向工程；

		逆向系统调用；
		借鉴别的软件；
		
```objectivec
1）增加新的功能模块
2）分析已有的功能模块
	a)修复Bug；
	b)修改定制；
	c)学习提高；
3）换个角度看待自己曾经熟悉的事物；（竞品分析）
```	
2.逆向工程常规套路：

	1）观察、猜测、寻找分析切入点；
	2）用dumpdecrypted给App砸壳；- 解密
	3）用class-dump导出Objective-C头文件；
	4）用Cycript定位目标视图；
	5）获取目标视图的UIViewCOntroller或delegate或model；
	6）在controller或delegate中寻找目标文件的蛛丝马迹；
	7）用hopper和lldb的组合还原调用逻辑；- 汇编
	8）...

3.逆向工程的一般过程		
系统分析:	
	
	目标程序的行为特征；
	文件组织架构；
代码分析:
	
	利用工具对程序本身的二进制文件进行分析；
4.逆向工程的工具			
监测工具:	
	
	Reveal - 查看目标App的UI元素；
	tcpdump
	libNotifyWatch
	PonyDebugger
	
开发工具:
	
	Xcode
	Theos - 开发越狱代码
反汇编器(disassembler):
	
	IDA - 横跨Windows/Linux/Mac平台，可执行文件输入，输入汇编代码或伪代码;
调试器(debugger):
	
	Xcode
	GDB(GNU Debugger)
	LLDB
二、越狱iOS平台简介		
1.iOS系统架构		
	
	在传统(非越狱)的iOS中，借助Xcode能访问var/mobile/Application下的App目录(Documents/Library/tmp)
	在越狱的iOS中，通过第三方文件管理工具(iFunBox、iExplorer、iFile等),访问iOS全系统文件.
2.iOS目录结构		
UNIX操作系统常见目录结构：

	/		根目录，以斜杆表示，其他所有文件和目录在根目录下展开；
	/bin	"binary"简写，存放提供用户级基础功能的二进制文件，如ls，ps等；
	/sbin	"system binary"的简写，存放提供系统级基础功能的二进制文件，如netstat、reboot等；
	/boot	存放能使系统成功启动的所有文件(这些文件一般在内核用户程序开始执行前调用，在iOS中此目录为空)；
	/dev	“device”的简写，存放BSD设备文件，每个文件代表系统的一个块设备(以块为单位传输数据，如硬盘)或字符设备(以字符为单位传输数据，如调制解调器)；
	/etc	“et cetera”的简写，存放系统脚本级配置文件，如passwd、hosts等。在iOS中，/etc是一个符号链接，实际指向 /private/etc；
	/lib	存放系统库文件、内核模块及设备驱动等，iOS中此目录为空；
	/mnt	“mount”的简写，存放临时的文件系统挂载点，iOS中此目录为空；
	/private	存放两个目录，分别是/private/etc和/private/var；
	/tmp	临时目录，iOS中，/tmp是一个符号链接，实际指向/private/tmp;
	/usr	包含大多数用户工具和程序;
			/usr/bin 包含那些/bin和/sbin中未出现的基础功能(nm、killall)；
			/usr/include 包含所有的标准C头文件；
			/usr/lib 存放库文件；
	/var	“variable”的简写，存放一些经常更改的文件，如日志、用户数据、临时文件等；
			其中/var/mobile/Applications下存放了所有App Store App；（重点关注）
iOS目录

	/Applications		存放所有的系统App和来自Cydia的App，不包括App Store App。
						(越狱的过程把/Applications变成一个符号链接，实际指向/var/stash/Applications) （重点关注）
	/Developer	   出现此目录是因为Xcode连接iOS设备时选择了“Use for Development”
	/Library			用了存放系统App数据。（最需要关注/Library/MobileSubstrate目录 - 存放了所有基于MobileSubstrate的插件）
		MobileSubstrate是一个提供hook功能的基础平台，运行在这个平台上的插件通常被称为tweak，/Library/MobileSubstrate下通常有3类文件：
			dylib	即Dynamic Library，也就是tweak插件；
			plist	用于配合dylib使用的filter文件，指定注入目标，格式：
						Filter = {
							Bundles = (com.apple.springboard);
						}
			disabled	被SBSetting禁用的tweak文件;(就是改个名字，不让MobileSubstrate加载)
	/System	包含大量的系统组件;(最重要的目录之一)
		/System/Library/Frameworks			存放iOS中的各种frameworks(公开+未公开)
		/System/Library/PrivateFrameworks	   存放iOS中的各种frameworks(公开+未公开)
		/System/Library/CoreServices	其中的SpringBoard.app也就是桌面管理器，是用户与系统交流的最重要的中介。
		/System/Library/PreferenceBundles	  其中各种bundle提供了“设置”中的绝大多数功能（iOS逆向入门的目标）
	/User	用户目录，实际指向/var/mobile
		/var/mobile/Media/DCIM		照片目录
		/var/mobile/Library/SMS		短信目录
		/var/mobile/Library/Mail	邮件目录
		/var/mobile/Library/CallHistory	通话记录
		/var/mobile/Applications 	存放通过App Store下载的App
3.iOS文件权限		
iOS是一个多用户操作系统.

4.iOS程序类型		
越狱iOS中最常见的程序：Application、Dynamic Library、Daemon     
Application

	bundle
		app
			(在越狱平台包含2类app：WeeApp-依附于Notification center的App 和 PreferenceBundle-依附于Setting的App)
		framework
			结构与app类似，但framework的bundle中存放的是一个动态链接库，不是可执行程序；
			app绝大多数功能都能 通过调用framework提供的接口来实现；
		PreferenceBundle
	App结构
		Info.plist  - 重要 
			记录app的基本信息(bundle identifier、可执行文件名、图标文件名等)
		Executable(可执行程序)
			app的执行入口，逆向工程最主要目标之一。
		Resources(资源文件)
			图标、图片、声音、配置文件、nib文件以及各种App运行时用到的资源文件。
			各种本地化字符串(.strings)是定位逆向目标的重要线索。
	/Applications  - 存放系统App及Cydia下载的App，属主用户和属主组一般时root和admin，sanbox限制小，多为deb格式
    /var/mobile/Applications 存放App Store 下载的App，属主用户和属主组都是mobile，sanbox限制大，多位ipa格式
		deb - Debian系统(包含Debian和Ubuntu)，配合APT软件管理系统使用
		ipa - 文件权限小，sanbox限制大，访问资源有限
		pxl - 起源于mac系统上pkg安装包
	sanbox
		文件访问
			sanbox会将app的文件访问限制在这个app的bundle内部；
			可以通过framework访问照片、歌曲、通讯录等系统数据；
		硬件调试
			可以通过framework来调用摄像头、GPS模块等硬件；
Dynamic Library
	
	Cydia中的tweak插件都是以Dynamic Library的形式工作的；
	Static Library:在app启动时，系统把app的代码和链接的Static Library全部放进分配好的内存空间，由于一次加载过多可能会造成App启动慢；
	Dynamic Library:在app启动时，iOS内核创建一个新进程，然后把app的代码加载到新进程的内存空间，同时内核还会启动Dynamic Loader(/usr/lib/dyld),把app需要的Dynamic Library加载进内存空间；
	权限和内存空间是由加载它的app决定的。

Daemon - 进程守护，Windows称Service
	
	后台运行，为用户使用操作系统提供各种“守护”(imagent保障iMessage的正确收发，mediaservered处理几乎所有音频、视频，syslogd记录系统日志等)；
	iOS中的daemon主要由：一个可执行文件+一个plist文件构成；
	iOS的根进程是/sbin/launchd,会在开机或接到命令时检查/System/Library/LaunchDaemons和/Library/Daemons下所有符合格式规定的plist文件(记录daemon的基本信息)，然后按需启动对应的daemon；
	
三、问题		
1.某个tweak不能写文件或通过objc_getClass函数获取不到类
	
	自己代码是否存在问题；
	权限问题导致；
	sanbox的误解导致；

		









参考资料：

1.iOS逆向工程-沙梓社(书籍)