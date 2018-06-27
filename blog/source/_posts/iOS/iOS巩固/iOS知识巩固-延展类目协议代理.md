---
title: iOS知识巩固-延展类目协议代理
date: 2018-03-13 22:04:47
tags: OC
category: iOS

---

1.代理delegate

<!--more-->

eg:

	创建一个类：NetworkFetcher
	NetworkFetcher.h
		@protocol NetworkFetcherDelegate <NSObject>
		
		@required //必须执行的方法
		
		- (void)printHello;
		
		@optional //可选执行的方法
		
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

定义:一种通用的设计模式

![delegate](delegate.png)

代理主要由三部分组成：

	协议：用来指定代理双方可以做什么，必须做什么。协议的内容一般是方法列表、属性。
	代理：根据指定的协议，完成委托方需要实现的功能。
	委托：根据指定的协议，指定代理去完成什么功能。

作用:实现消息传递(消息传递的其他方式:通知、KVO、Block、属性等)

优点:
	
	 非常严格的语法。所有将听到的事件必须是在delegate协议中有清晰的定义;
     如果delegate中的一个方法没有实现那么就会出现编译警告/错误;
     协议必须在控制器的作用域范围内定义;
     在一个应用中的控制流程是可跟踪的并且是可识别的；
     在一个控制器中可以定义定义多个不同的协议，每个协议有不同的delegates;
     没有第三方对象要求保持/监视通信过程;
     能够接收调用的协议方法的返回值。这意味着delegate能够提供反馈信息给控制器;
缺点:

	需要定义很多代码：a.协议定义；b.控制器的delegate属性；c.在delegate本身中实现delegate方法定义
    在释放代理对象时，需要小心的将delegate改为nil。一旦设定失败，那么调用释放对象的方法将会出现内存crash
    在一个控制器中有多个delegate对象，并且delegate是遵守同一个协议，但还是很难告诉多个对象同一个事件，不过有可能;

2.协议protocol



3.类目catigory

为已知的类添加新的方法，但是不能添加实例变量；类目和类只是语法上稍显不同。

格式：”类名+扩展名“
	
	@interface NSString(CompareWithValue)
	...
	@end

	@implementation NSString(CompareWithValue)
	...
	@end
作用：
	
	为已知的类增加方法，方法对外可见；
	当现有类方法无法满足需要时，可以使用类目扩展一个类，增加方法；
应用：	

	对现有的类进行扩展（系统中的类，在类目中增加的方法会被子类继承，在运行时跟其他方法无区别）；
	作为子类的替代方式，不需要定义和使用一个子类，可以通过类目直接向已有的类里增加方法；
	对类中的方法进行归类，利用类目把一个庞大的类划分小块来分别进行开发，从而更好地对类中的方法进行更新和维护。
	和普通接口有所区别：在类目的实现文件中的实例方法只要你不去调用它，你可以不用实现所有声明方法。
局限性：
	
	无法向类目中添加新的实例变量；
	类目中的方法具有更高的优先级，若在类目中覆盖原始类的方法(重载)，会引起super消息的无效。(因此，一般不要覆盖现有类中的方法)如果确实要重载，那就通过继承创建子类来实现；
4.延展extension

格式:
	
	形式和类目相同，不用新创建文件，在类的实现文件(.m)中加入
	@interface 
	...
	@end
作用：
	
	增加方法，让外部不可见(定义自己的私有方法)。
应用：
	
	对于想要隐藏的算法和API，可以使用延展；
	方便代码的书写(每次声明一个全局变量或属性都会到.h文件，此时使用延展更方便);
tips:当在定义延展的时候不提供类目名时，延展中定义的方法即被视为“必须实现”的API，在此情况下，如果没有实现代码，编译器会报警告。



参考资料：

1.[代理设计模式](http://www.cocoachina.com/ios/20160317/15696.html)<br>
2.[iOS 协议(protocol)和代理(delegate)模式详解](https://www.jianshu.com/p/4abaf1d1f044)
