---
title: iOS逆向-ObjectiveC相关的iOS逆向基础
date: 2018-05-28 13:29:15
tags: 逆向
categories: iOS
---
 
iOS采用文件格式 Mach-O 中包含了足够多的metadata，通过class-dump 等工具还原类的@interface<br>
一、tweak的作用原理<br>  
在iOS中，tweak指那些能够增强其他执行程序功能的动态链接库，是越狱iOS的重要组成部分。<br>

<!--more-->
 
	绝大部分tweak都是基于MobileSubstrate 编写的；
1.MobileSubstrate

	组成:MobileHooker、MobileLoader、Safe mode
MobileHooker
	
	作用：替换系统函数，即所谓的hook技术
	主要函数:
		
		IMP MSHookMessage(Class class,SEL selector,IMP replacement,const char* prefix);
		说明：此方法不是线程安全，已弃用，MSHookMessageEx为替换方法；
		
		void MSHookMessageEx(Class class,SEL selector,IMP replacement,IMP *result);
		说明：通过调用IMP method_setImplementation(Method method,IMP imp)来将[class selector]的实现 改为 replacement; 
		
		void MSHookFunction(void* function,void* replacement,void** p_original);
		说明:用于c/c++函数，通过汇编指令，在程序执行到被hook的位置时，跳转到replacement处，并分配一块内存来保护被hook而没有得到执行的指令和其返回地址，保证程序能够在执行完成replacement后继续运行；
MobileLoader

	作用:加载第三方动态链接库，即tweak；
	原理：iOS启动时，由launchd 将MobileLoader载入内存，MobileLoader会dlopen 所有/Library/MobileSub-strate/DynamicLibraries/目录下的动态链接库；
	配置文件：
		编写一个与动态链接库名字相同的配置文件，以plist作为扩展名，指定动态链接库的作用范围；
Safe mode

	作用:保障程序的顺利执行，用于用户查错修复程序；
	tweak是动态链接库，寄生在别的可执行程序中，为了防止Crash，引入Safe mode；
	捕获信号：
		SIGTRAP
		SIGABRT
		SIGILL
		SIGBUS
		SIGSEGV
		SIGSYS
	原理：当捕获到上述6种信号，进入安全模式，在安全模式中，所有第三方插件均被禁用。
2.编写tweak

	语言：C、C++、Objective-C
	1.发现需求
		多使用、多观察
		倾听用户的声音
		解剖iOS
	2.实现需求
		分析文件、寻找切入点
		
 	
 
 参考资料:		
 1.[iOS工具 - Cydia](http://cydia.radare.org/)<br>
 2.[砸壳 - dumpdecrypted](https://github.com/stefanesser/dumpdecrypted/archive/master.zip)