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
   [yololib - 动态注入](https://github.com/KJCracks/yololib)<br>
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
		用xcode打开下载的insert_dylib项目，并编译，通过Xcode中的Products下 选中insert_dylib 右键 show in Finder  
		将 insert_dylib 脚本拷贝到 /usr/local/bin 目录中
			不放到此目录中需要使用./insert_dylib，
			放在目录中后只需要使用insert_dylib
	
	步骤：
		1.先将动态库ModifyStepCount.dylib 复制到 xx/Playload/WeChat.app 中;
		2.cd 到需要注入的包中：
			eg: cd xxx/xxx/Desktop/WeChat/Payload/WeChat.app/
		3.命令:
			insert_dylib @executable_path/ModifyStepCount.dylib WeChat
		4.会生成 WeChat_patched 二进制文件
			
			Binary is a fat binary with 2 archs.
			LC_CODE_SIGNATURE load command found. Remove it? [y/n] n
			LC_CODE_SIGNATURE load command found. Remove it? [y/n] n
		
6.用[MachOView 工具](https://sourceforge.net/projects/machoview/) 查看新的动态库是否已经被注入了
	
	File --> Open --> xxx/WeChat/Payload/WeChat.app/Wechat_patched
	Fat Binary --> Executable --> Load Commands --> LC_LOAD_DYLIB(ModifyStepCount.dylib)
7.修改 WeChat_patched -->WeChat 替换原来的文件，然后和ModifyStepCount.dylib 动态库一起放入WeChat.app中
	
	在菜单栏上 File-->Open-->xxx/Payload/WeChat.app/WeChat
8.用[iOS App Signer - 工具](https://github.com/DanTheMan827/ios-app-signer)重签名并打包

	用xcode打开 iOS App Signer，选择证书和签名
		可以新创建一个 个人账号生成证书 的 项目，用此证书重新签名
	在iOS App Singer 上执行相关操作后生成一个IPA包，安装在手机上即可。
五、微信防撤回<br>
	
	#pragma mark - 微信防撤回
	CHOptimizedMethod(1, self, void, CMessageMgr, onRevokeMsg, id, value1) {
	    
	}
	#pragma mark - 微信步数
	CHDeclareClass(WCDeviceStepObject); // declare class
	
	CHOptimizedMethod(0, self, unsigned long, WCDeviceStepObject, m7StepCount)
	{
	    return 151207;
	}
	CHOptimizedMethod(0, self, unsigned long, WCDeviceStepObject, hkStepCount)
	{
	    return 151207;
	}
	
	__attribute__((constructor)) static void entry()
	{
	    //加载CMessageMgr类
	    CHLoadLateClass(CMessageMgr);
	    //hook AsyncOnAddMsg:MsgWrap:方法
	    CHClassHook(2, CMessageMgr, AsyncOnAddMsg, MsgWrap);
	    
	    // 微信步数  CHLoadLateClass(WCDeviceStepObject);
	    CHHook(0, WCDeviceStepObject, m7StepCount);
	    CHHook(0, WCDeviceStepObject, hkStepCount);
	    
	    // 消息防撤回
	    CHHook(1, CMessageMgr, onRevokeMsg);
	}
六、微信红包<br>

	#pragma mark - 微信红包
	
	/**
	 *  插件功能
	 */
	static int const kCloseRedEnvPlugin = 0;
	static int const kOpenRedEnvPlugin = 1;
	static int const kCloseRedEnvPluginForMyself = 2;
	static int const kCloseRedEnvPluginForMyselfFromChatroom = 3;
	
	//0：关闭红包插件
	//1：打开红包插件
	//2: 不抢自己的红包
	//3: 不抢群里自己发的红包
	static int HBPliginType = 1;
	
	#define SAVESETTINGS(key, value) { \
	NSArray *paths = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES); \
	NSString *docDir = [paths objectAtIndex:0]; \
	if (!docDir){ return;} \
	NSMutableDictionary *dict = [NSMutableDictionary dictionary]; \
	NSString *path = [docDir stringByAppendingPathComponent:@"HBPluginSettings.txt"]; \
	[dict setObject:value forKey:key]; \
	[dict writeToFile:path atomically:YES]; \
	}
	
	//#define LOADSETTINGS(key) ({ \
	//NSArray *paths = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES); \
	//NSString *docDir = [paths objectAtIndex:0]; \
	//if (!docDir){ return} \
	//NSString *path = [docDir stringByAppendingPathComponent:@"HBPluginSettings.txt"]; \
	//NSDictionary *dict = [[NSDictionary alloc] initWithContentsOfFile:path]; \
	//if(!dict){ return} \
	//NSNumber *number = [dict objectForKey:key]; \
	//0
	//})
	
	CHDeclareClass(CMessageMgr);
	
	CHMethod(2, void, CMessageMgr, AsyncOnAddMsg, id, arg1, MsgWrap, id, arg2)
	{
	    CHSuper(2, CMessageMgr, AsyncOnAddMsg, arg1, MsgWrap, arg2);
	    Ivar uiMessageTypeIvar = class_getInstanceVariable(objc_getClass("CMessageWrap"), "m_uiMessageType");
	    ptrdiff_t offset = ivar_getOffset(uiMessageTypeIvar);
	    unsigned char *stuffBytes = (unsigned char *)(__bridge void *)arg2;
	    NSUInteger m_uiMessageType = * ((NSUInteger *)(stuffBytes + offset));
	    
	    Ivar nsFromUsrIvar = class_getInstanceVariable(objc_getClass("CMessageWrap"), "m_nsFromUsr");
	    id m_nsFromUsr = object_getIvar(arg2, nsFromUsrIvar);
	    
	    Ivar nsContentIvar = class_getInstanceVariable(objc_getClass("CMessageWrap"), "m_nsContent");
	    id m_nsContent = object_getIvar(arg2, nsContentIvar);
	    
	    switch(m_uiMessageType) {
	        case 1:
	        {
	            //普通消息
	            //红包插件功能
	            //0：关闭红包插件
	            //1：打开红包插件
	            //2: 不抢自己的红包
	            //3: 不抢群里自己发的红包
	            //微信的服务中心
	            Method methodMMServiceCenter = class_getClassMethod(objc_getClass("MMServiceCenter"), @selector(defaultCenter));
	            IMP impMMSC = method_getImplementation(methodMMServiceCenter);
	            id MMServiceCenter = impMMSC(objc_getClass("MMServiceCenter"), @selector(defaultCenter));
	            //通讯录管理器
	            id contactManager = ((id (*)(id, SEL, Class))objc_msgSend)(MMServiceCenter, @selector(getService:),objc_getClass("CContactMgr"));
	            id selfContact = objc_msgSend(contactManager, @selector(getSelfContact));
	            
	            Ivar nsUsrNameIvar = class_getInstanceVariable([selfContact class], "m_nsUsrName");
	            id m_nsUsrName = object_getIvar(selfContact, nsUsrNameIvar);
	            BOOL isMesasgeFromMe = NO;
	            if ([m_nsFromUsr isEqualToString:m_nsUsrName]) {
	                //发给自己的消息
	                isMesasgeFromMe = YES;
	            }
	            
	            if (isMesasgeFromMe)
	            {
	                if ([m_nsContent rangeOfString:@"打开红包插件"].location != NSNotFound)
	                {
	                    HBPliginType = kOpenRedEnvPlugin;
	                }
	                else if ([m_nsContent rangeOfString:@"关闭红包插件"].location != NSNotFound)
	                {
	                    HBPliginType = kCloseRedEnvPlugin;
	                }
	                else if ([m_nsContent rangeOfString:@"关闭抢自己红包"].location != NSNotFound)
	                {
	                    HBPliginType = kCloseRedEnvPluginForMyself;
	                }
	                else if ([m_nsContent rangeOfString:@"关闭抢自己群红包"].location != NSNotFound)
	                {
	                    HBPliginType = kCloseRedEnvPluginForMyselfFromChatroom;
	                }
	                
	                SAVESETTINGS(@"HBPliginType", [NSNumber numberWithInt:HBPliginType]);
	            }
	        }
	            break;
	        case 49: {
	            // 49=红包
	            
	            //微信的服务中心
	            Method methodMMServiceCenter = class_getClassMethod(objc_getClass("MMServiceCenter"), @selector(defaultCenter));
	            IMP impMMSC = method_getImplementation(methodMMServiceCenter);
	            id MMServiceCenter = impMMSC(objc_getClass("MMServiceCenter"), @selector(defaultCenter));
	            //红包控制器
	            id logicMgr = ((id (*)(id, SEL, Class))objc_msgSend)(MMServiceCenter, @selector(getService:),objc_getClass("WCRedEnvelopesLogicMgr"));
	            //通讯录管理器
	            id contactManager = ((id (*)(id, SEL, Class))objc_msgSend)(MMServiceCenter, @selector(getService:),objc_getClass("CContactMgr"));
	            
	            Method methodGetSelfContact = class_getInstanceMethod(objc_getClass("CContactMgr"), @selector(getSelfContact));
	            IMP impGS = method_getImplementation(methodGetSelfContact);
	            id selfContact = impGS(contactManager, @selector(getSelfContact));
	            
	            Ivar nsUsrNameIvar = class_getInstanceVariable([selfContact class], "m_nsUsrName");
	            id m_nsUsrName = object_getIvar(selfContact, nsUsrNameIvar);
	            BOOL isMesasgeFromMe = NO;
	            BOOL isChatroom = NO;
	            if ([m_nsFromUsr isEqualToString:m_nsUsrName]) {
	                isMesasgeFromMe = YES;
	            }
	            if ([m_nsFromUsr rangeOfString:@"@chatroom"].location != NSNotFound)
	            {
	                isChatroom = YES;
	            }
	            if (isMesasgeFromMe && kCloseRedEnvPluginForMyself == HBPliginType && !isChatroom) {
	                //不抢自己的红包
	                break;
	            }
	            else if(isMesasgeFromMe && kCloseRedEnvPluginForMyselfFromChatroom == HBPliginType && isChatroom)
	            {
	                //不抢群里自己的红包
	                break;
	            }
	            
	            if ([m_nsContent rangeOfString:@"wxpay://"].location != NSNotFound)
	            {
	                NSString *nativeUrl = m_nsContent;
	                NSRange rangeStart = [m_nsContent rangeOfString:@"wxpay://c2cbizmessagehandler/hongbao"];
	                if (rangeStart.location != NSNotFound)
	                {
	                    NSUInteger locationStart = rangeStart.location;
	                    nativeUrl = [nativeUrl substringFromIndex:locationStart];
	                }
	                
	                NSRange rangeEnd = [nativeUrl rangeOfString:@"]]"];
	                if (rangeEnd.location != NSNotFound)
	                {
	                    NSUInteger locationEnd = rangeEnd.location;
	                    nativeUrl = [nativeUrl substringToIndex:locationEnd];
	                }
	                
	                NSString *naUrl = [nativeUrl substringFromIndex:[@"wxpay://c2cbizmessagehandler/hongbao/receivehongbao?" length]];
	                
	                NSArray *parameterPairs =[naUrl componentsSeparatedByString:@"&"];
	                
	                NSMutableDictionary *parameters = [NSMutableDictionary dictionaryWithCapacity:[parameterPairs count]];
	                for (NSString *currentPair in parameterPairs) {
	                    NSRange range = [currentPair rangeOfString:@"="];
	                    if(range.location == NSNotFound)
	                        continue;
	                    NSString *key = [currentPair substringToIndex:range.location];
	                    NSString *value =[currentPair substringFromIndex:range.location + 1];
	                    [parameters setObject:value forKey:key];
	                }
	                
	                //红包参数
	                NSMutableDictionary *params = [@{} mutableCopy];
	                
	                [params setObject:parameters[@"msgtype"]?:@"null" forKey:@"msgType"];
	                [params setObject:parameters[@"sendid"]?:@"null" forKey:@"sendId"];
	                [params setObject:parameters[@"channelid"]?:@"null" forKey:@"channelId"];
	                
	                id getContactDisplayName = objc_msgSend(selfContact, @selector(getContactDisplayName));
	                id m_nsHeadImgUrl = objc_msgSend(selfContact, @selector(m_nsHeadImgUrl));
	                
	                [params setObject:getContactDisplayName forKey:@"nickName"];
	                [params setObject:m_nsHeadImgUrl forKey:@"headImg"];
	                [params setObject:[NSString stringWithFormat:@"%@", nativeUrl]?:@"null" forKey:@"nativeUrl"];
	                [params setObject:m_nsFromUsr?:@"null" forKey:@"sessionUserName"];
	                
	                if (kCloseRedEnvPlugin != HBPliginType) {
	                    //自动抢红包
	                    ((void (*)(id, SEL, NSMutableDictionary*))objc_msgSend)(logicMgr, @selector(OpenRedEnvelopesRequest:), params);
	                }
	                return;
	            }
	            
	            break;
	        }
	        default:
	            break;
	    }
	}

七、安装iOSOpenDev失败的解决方法	
	
	先重启Xcode，在Xcode中创建新项目中，查看菜单iOS中是否存在iOSOpenDev 一项内容	 
1.[官方解决方法](https://github.com/kokoabim/iOSOpenDev/wiki/Troubleshoot)<br>
	
	1.在点击 安装iOSOpenDev软件的失败弹框 任意地方，使其成为Mac菜单即可;
	2.Command + L 显示安装日志弹框;
	3.在弹框的左上角选择 "显示所有日志";
	4.滚动到日志底部，查看失败原因;
	5.根据原因处理问题;
2.其他解决方法

八、其他		
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
8.[yololib - 动态注入](https://github.com/KJCracks/yololib)<br>
9.[insert_dylib的使用方法](https://www.jianshu.com/p/5d353d6db145)<br>