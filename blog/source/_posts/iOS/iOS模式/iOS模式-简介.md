---
title: iOS模式-简介
toc: true
date: 2018-06-27 13:35:20
tags: 模式
categories: iOS
---

一、设计模式、架构模式、框架

<!-- more -->

1.设计模式
	
	对某种环境中反复出现的问题以及解决该问题的方案的描述规范；
	增加代码可重用性，简洁可靠；
	eg：代理模式、迭代器模式、策略模式等；
2.架构模式

	为管理复杂的应用程序而出现的一种模式；
	eg: MVC架构、MVVM架构；
3.框架

	通常是代码的重用；
	为已经解决问题的具体实现方法，能直接执行或复用；
	一个框架中往往包含一种或多种设计模式；
	eg：Foundation、UIKit、AFNetworking、MJRefresh等；
	
4.区别

	设计模式 比 框架 更抽象；
	设计模式 比 框架 更小的元素；
二、设计模式<br>
1.分类:<br>

	创建型
		...
		单例模式 - 避免一个全局使用的类频繁的创建和销毁；
		工厂模式 - 解决接口选择的问题，创建过程延迟到子类进行；
		...
	结构型
		...
		代理模式 - 为其它对象提供一种代理以控制对这个对象的访问；
		...
	行为型
		...
		观察者模式 - 解决一个对象改变状态给其它对象通知的问题；
		策略模式 - 定义一系列算法，把他们封装起来，并且使他们可以相互替换；
![mode]( mode.png)<br>
三、观察者模式<br>
1.解决问题
	
	解决了一个对象改变状态给其它对象通知的问题；
2.解决方案
	
	经典观察者模式
		当 A 对 B 的变化感兴趣，需要监听 B 的状态变化，就注册为 B 的观察者，当 B 发生变化时通知 A，告诉 A 此时 B 发生了变化，A 根据 B 的变化做相应的操作响应；
	eg: KVO、Delegate、NSNotificationCenter等
2.具体实现
eg1:KVO举例
	
	注册观察者
	//观察属性name
    [_myName addObserver:self forKeyPath:@"name" options:NSKeyValueObservingOptionNew || NSKeyValueChangeOldKey context:nil];
    //观察属性myView
    [self addObserver:self forKeyPath:@"myView" options:NSKeyValueObservingOptionNew || NSKeyValueChangeOldKey context:nil];

	
	响应操作
	//一旦属性被操作了，这里会自动响应（上面设置观察的属性才会在这响应）
	- (void)observeValueForKeyPath:(NSString *)keyPath ofObject:(id)object change:(NSDictionary<NSString *,id> *)change context:(void *)context {
	    if ([keyPath isEqualToString:@"name"]) {
	        ...
	    } 
	    if ([keyPath isEqualToString:@"myView"]) {
	        ...
	    }
	}
	移除观察者
	- (void)dealloc {
	    [_myName removeObserver:self forKeyPath:@"name"];
	    [self removeObserver:self forKeyPath:@"myView"];
	}
eg2:NSNotificationCenter

	注册观察者
	[[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(didRotate:) name:@"registerKVO" object:nil];

	响应操作
	//解析消息内容
	- (void)didRotate:(UIInterfaceOrientation)fromInterfaceOrientation
	{
	   ...
	}
	移除观察者
	[[NSNotificationCenter defaultCenter] removeObserver:self];
四、单例模式<br>
1.解决问题
	
	避免一个全局使用的类频繁的创建和销毁；
2.解决方案：

	eg1：GCD - 线程安全
	创建一个全局单例
	+ (Singleton *)sharedInstance{
	    static id instance = nil;
	    static dispatch_once_t onceToken;
	    dispatch_once(&onceToken, ^{
	        instance = [[self alloc] init];
	    });
	    return instance;
	}
    
    eg2:单例宏定义
	创建一个 A 类
	A.h
		#undef	AS_SINGLETON
		#define AS_SINGLETON( __class ) \
		+ (__class *)sharedInstance;
		
		#undef	DEF_SINGLETON
		#define DEF_SINGLETON( __class ) \
		+ (__class *)sharedInstance{ \
		static dispatch_once_t once; \
		static __class * __singleton__; \
		dispatch_once( &once, ^{ \
		__singleton__ = [[__class alloc] init]; \
		} ); \
		return __singleton__; \
		}
	创建单例类 B
	B.h
		@interface B : NSObject
		...
		AS_SINGLETON(B)
		...
		@end
	B.m
		@implementation B
		...
		DEF_SINGLETON(B)
		...
		@end
五、Delegate模式<br>
1.解决问题
	
	为其它对象提供一种代理以控制对这个对象的访问；
2.解决方案
	
	创建一个类：NetworkFetcher
	NetworkFetcher.h
		@protocol NetworkFetcherDelegate <NSObject>
		
		@required
		- (void)printHello;
		
		@optional
		- (void)didReceiveData:(NSData *)data;
		- (void)didFailWithError:(NSError *)error;
		- (void)didUpdateProgressTo:(CGFloat)progress;
		
		@end
		
		@interface GRHNetworkFetcher : NSObject
		
		@property (nonatomic, assign)id<NetworkFetcherDelegate> delegate;
		
		@end
	NetworkFetcher.m
		//不实现协议方法
	
	创建一个使用协议方法的类：A
	A.m
		- (void)doSometing {
			if ([self.delegate respondsToSelector:@selector(didReceiveData:)]) {
				[self.delegate didReceiveData:nil];
			}
		}
		解析：
			respondsToSelector: 来判断委托对象是否实现了相关方法。如果实现了，就调用；如果没有实现，就不执行任何操作；

参考资料:<br>
1.[设计模式](https://www.jianshu.com/p/afe8e0c6362f)<br>
2.[iOS Delegate模式性能优化](https://www.jianshu.com/p/dd84eaaff115)<br>