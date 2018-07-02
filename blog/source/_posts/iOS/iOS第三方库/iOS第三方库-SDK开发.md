---
title: iOS第三方库-SDK开发
date: 2018-05-17 22:44:08
tags: iOS第三方库
categories: iOS
---

一、SDK开发准备<br>
1.规则:
	
	文件组织方式清晰明了；
	类名前缀和包命令或缩写要一致；
	代码风格一定要一致；
	函数命令遵循共性，不要出现歧义或违背常识；
	代码注释规范、清楚；
	SDK功能正确，编译无警告、错误，支持最新特性；
2.设计:
	
	易用
		SDK集成成本： cocoapods；
		API调用简单；
		功能可以定制；
		便于调试： debug 日志；
		API回调参数明确；
		API稳定；
	API设计
		参数命名明确无歧义；
		自足自给(SDK自己获取如BundleID、appName、appVersion等信息)；
		SDK配置参数、接入参数分开；
		SDK参数:拼接的字符串如支付宝的支付orderString；
		同一类参数，封装成model，隐藏属性，通过方法构造；
		API功能单一，减少类似enum 的入参设计；
		用于查询的属性，绝对不能直接设置；
		API回调:delegate 和 block
3.工程搭建<br>
建议使用cocoapods和[cocoapods-packager](https://github.com/CocoaPods/cocoapods-packager)
	
	搭建开发工具:
		pod lib create xxx   #xxx为工程名
	打包:
		pod package xxx.spec
	
	SDK打包形式：
		Static Library
		Dynamic Framework
		Universal Framework
	版本控制:
		(major).(minor).(patch)
		eg:3.0.1
	发布渠道:
		cocoapods
		邮件
		网站
4.注意事项：

	减少对其他库的依赖：能用系统API解决就不用第三方；
	OC没有命名空间，类命名和类别方法加上前缀；
	多考虑第三方带来的影响: 键盘处理、UIKit的UIAppearance等；
	依赖其他SDK的，别打包在一起，否则出现符号表重复；
	使用OC类别打包时，加上-ObjC；
	尽量不使用 单例；
	核心代码安全性；
	资源文件使用 bundle 进行管理，尽量不使用Xib；
二、创建静态库

三、创建framework


四、打包(Bundle)资源






参考资料:		
1.[SDK开发规则](https://www.jianshu.com/p/dd8d1b7ce1e4)
1.[SDK开发](https://www.jianshu.com/p/c131baae4307)    
2.[创建SDK](https://www.jianshu.com/p/65b1c1326c50)
