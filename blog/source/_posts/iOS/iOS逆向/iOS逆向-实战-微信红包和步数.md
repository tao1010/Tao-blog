---
title: iOS逆向-实战-微信红包和步数
date: 2018-06-08 13:31:12
tags: 逆向
categories: iOS
---

一、工具和软件		
1.[PP助手Mac版](https://pro.25pp.com/pp_mac_ios)<br>
2.iOS微信App(越狱) 6.6.7 - Mac PP助手下载<br>
3.[dumpcrypted - 砸壳](https://github.com/stefanesser/dumpdecrypted)<br>
4.[class-dump - 导出头文件](https://github.com/nygard/class-dump)<br>
5.[iOSOpenDev1.6-2 - 替换方法](http://www.iosopendev.com)<br>
6.[insert_dylib - 动态注入方法](https://github.com/Tyilo/insert_dylib)<br>
7.[MachOView - 查看注入是否成功](https://sourceforge.net/projects/machoview/)<br>
8.[iOS App Signer - 签名工具](https://github.com/DanTheMan827/ios-app-signer)

二、资源和环境		
1.macOS Hign Sierra 10.13.4<br>
2.iOS11.2.1 和 iOS11.3.1 <br>
3.Xcode9.3.1<br>


三、操作步骤			
1.安装Mac PP助手;		<br>
2.在PP助手中下载越狱的微信app(在未连接手机的状态下在);  <br>

	如果是从appstore上下载微信app，由于此时的app是经过加密的,则需要先进行砸壳;
	如果是越狱的微信App,则不需要砸壳;
3.拷贝一份微信App,修改其后缀名：.ipa-->.zip,解压ZIP文件;<br>
4.安装class-dump工具;<br>
	
	在命令终端输入:class-dump 可查看class-dump软件的使用命令;
5.使用class-dump指令获取微信头文件;<br>

	命令终端输入:
		class-dump -H xxxx(上述步骤3中解压的WeChat文件)/Payload/WeChat.app -o xxx(新建的文件夹-用于导出微信头文件)/WeChatHeaders
		eg:
		class-dump -H /Users/jingxiankeji_tao/Desktop/WeChat/Payload/WeChat.app -o /Users/jingxiankeji_tao/Desktop/WeChatHeaders 
6.检查头文件是否导出正确:
	
	查看导出的头文件夹:
		若CDStructures.h中无数据：
			1.还需要砸壳，工具有：AppCrackr、Clutch、dumpcrypted等，推荐dumpcrypted；
			2.当砸壳完毕后，使用 class-dump 仍然只导出 CDStructures.h一个文件，则可能架构选择错误，armv7对应的是iPhone5及以下的设备，arm64则是5s及以上的设备，所以微信也包含两个架构，armv7和arm64,使用如下命令:
				class-dump --arch armv7 xxxx/Payload/WeChat.app -H -o xxx/WeChatHeaders
		若存在11695个文件，则完成头文件的导出步骤;
7.定位相关的文件(步数或红包)<br>
四、微信步数<br>		
1.定位于步数相关的文件<br>
	
	搜索头文件夹下的包含‘step’的文件
		WCDeviceStepObject.h
2.分析文件方法:用Xcode打开WCDeviceStepObject.h文件
	
	#import "MMObject.h"
	
	@class NSMutableArray;
	
	@interface WCDeviceStepObject : MMObject
	{
	    ...
	    unsigned int m7StepCount;
	    unsigned int hkStepCount;
	    ...
	}
	
	...
	@property(nonatomic) unsigned int hkStepCount; // @synthesize hkStepCount;
	@property(nonatomic) unsigned int m7StepCount; // @synthesize m7StepCount;
	...
	
	@end
	
	分析：
		此处的两个属性get方法：判断HealthKit是否可用，并去获取其中数据;
		替换两个get方法即可修改微信运动的步数;
3.使用工具iOSOpenDev替换方法
	
![iosopendev](iosopendev.png)<br>
![fixstep1](fixstep1.png)<br>

	1.利用iOSOpenDev创建hook模版
		打开Xcode，进入创建一个新项目的菜单中，选择iOS-->iOSOpenDev --> CaptainHookTweak
	2.在代码中修改上述的两个get方法，返回想要的数值
		在创建的项目XXX.mm文件中，具体修改如下:
			
		@class WCDeviceStepObject;

		CHDeclareClass(WCDeviceStepObject); // declare class
		
		CHOptimizedMethod(0, self, unsigned long, WCDeviceStepObject, m7StepCount)
		{
		    return 20151207;
		}
		CHOptimizedMethod(0, self, unsigned long, WCDeviceStepObject, hkStepCount)
		{
		    return 20151207;
		}
		
		CHConstructor // code block that runs immediately upon load
		{
			@autoreleasepool
			{
		        CHLoadLateClass(WCDeviceStepObject);
				CHHook(0, WCDeviceStepObject, m7StepCount); // register hook
		        CHHook(0, WCDeviceStepObject, hkStepCount); // register hook
			}
	}
4.编译Xcode创建的项目ModifyStepCount<br>
![dylib](dylib.png)<br>

	build xcode 生成 .dylib	
5.用[insert_dylib工具](https://github.com/Tyilo/insert_dylib) 动态库的地址注入Mach-O

	insert_dylib使用方法：
		insert_dylib dylib_path binary_path [new_binary_path]
	
	步骤：
		1.先将动态库ModifyStepCount.dylib 复制到 xx/Playload/WeChat.app 中;
		2.使用insert_dylib:将脚本拉入命令行 输入命令:
			insert_dylib --all-yes @executable_path/test.dylib Payload/WeChat.app/WeChat
			或
			optool install -c load -t "@executable_path/test.dylib" -p Payload/WeChat.app/WeChat	 
		
		
	
6.用[MachOView 工具](https://sourceforge.net/projects/machoview/) 查看新的动态库是否已经被注入了
	
7.修改 WeChat_patched -->WeChat 替换原来的文件，然后和ModifyStepCount.dylib 动态库一起放入WeChat.app中
	
	在菜单栏上 File-->Open-->xxx/Payload/WeChat.app/WeChat
8.用[iOS App Signer - 工具](https://github.com/DanTheMan827/ios-app-signer)重签名并打包

	用xcode打开，选择证书和签名
		
	
五、微信红包<br>


六、安装iOSOpenDev失败的解决方法	
	
	先重启Xcode，在Xcode中创建新项目中，查看菜单iOS中是否存在iOSOpenDev 一项内容	 
1.[官方解决方法](https://github.com/kokoabim/iOSOpenDev/wiki/Troubleshoot)<br>
	
	1.在点击 安装iOSOpenDev软件的失败弹框 任意地方，使其成为Mac菜单即可;
	2.Command + L 显示安装日志弹框;
	3.在弹框的左上角选择 "显示所有日志";
	4.滚动到日志底部，查看失败原因;
	5.根据原因处理问题;
2.其他解决方法

七、其他		
1.iOS可执行文件位置：
	
	一般我们得到的iOS程序包是.ipa文件。其实就是一个压缩包，解压缩.ipa。解压缩后里面会有一个payload文件夹，文件夹里有一个.app文件，右键显示包内容，然后找到一个一般体积最大跟.app同名的文件，那个文件就是可执行文件；
	


参考资料：			
1.[非越狱微信步数和红包](https://www.jianshu.com/p/7c0c2bcbbaf2)	<br>
2.[非越狱iOS微信自动抢红包](https://www.jianshu.com/p/189afbe3b429)	<br>
3.[非越狱iOS微信自动抢红包2](https://www.jianshu.com/p/ad578bef4b76)<br>
4.[非越狱iOS修改微信步数](https://www.jianshu.com/p/dd600ee4d659)	<br>
5.[安装iOSOpenDev失败的解决方法](https://www.ianisme.com/ios/2319.html)<br>
6.[在非越狱平台进行越狱开发](https://blog.csdn.net/zcrong/article/details/51619381)<br>
7.[iOS可执行文件分析工具MachoOView](https://www.jianshu.com/p/6c45da26040d)<br>