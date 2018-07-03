---
title: iOS面试-OC
toc: true
date: 2018-07-03 21:52:01
tags: iOS面试
categories: iOS
---

1.#import、#include、@class的区别

<!-- more -->

	#import和#include都能完整地包含某个文件的内容;
	#import能防止同一个文件被包含多次;
	
	#import <> 用来包含系统自带的文件或第三方静态库文件;
	#import “”用来包含自定义的文件;
	
	@class仅仅是声明一个类名，并不会包含类的完整声明;
	@class还能解决循环包含的问题;
2.readwrite，readonly，assign，retain，copy，nonatomic的作用

	readwrite：同时生成get方法和set方法的声明和实现;
	readonly：只生成get方法的声明和实现;
	assign：set方法的实现是直接赋值，用于基本数据类型;
	retain：set方法的实现是release旧值，retain新值，用于OC对象类型;
	copy：set方法的实现是release旧值，copy新值，用于NSString、block等类型;
	nonatomic：非原子性，set方法的实现不加锁，不安全，性能高;
	atomic：性能低，通过锁定机制来确保其原子性,但只是读/写安全,不能绝对保证线程的安全，当多线程同时访问的时候，会造成线程不安全。可以使用线程锁来保证线程的安全;
3.setter、getter方法实现

	@property （nonatomic,retain）NSString *name
	- (void)setName:(NSString *)name{
		if(_name != name){
			[_name release];
			_name = [name retain];
		}
	}
	
	@property （nonatomic,copy）NSString *name
	- (void)setName:(NSString *)name{
		if(_name != name){
			[_name release];
			_name = [name copy];
		}
	}
4.NSString *obj = [[NSData alloc] init]; 编译时和运行时的状态
	
	编译时 NSString 类型
	运行时 NSData 类型
5.id声明的变量有什么特性
	
	id声明的变量能指向任何OC对象
6.OC对内存的管理
	
	每个对象都有一个引用计数器，每个新对象的计数器是1，当对象的计数器减为0时，就会被销毁
	通过retain可以让对象的计数器+1、release可以让对象的计数器-1
	还可以通过autorelease pool管理内存
	如果用ARC，编译器会自动生成管理内存的代码
	
	注意：不管是MRC还是ARC都是在编译时完成的
7.MRC中的内存管理
	
	谁开辟谁管理，谁引用谁释放；
	
	NSMutableArray* ary = [[NSMutableArray array] retain];
    NSString *str = [NSString stringWithFormat:@"test"];
    [str retain];
    [ary addObject:str];
    NSLog(@"%ld", (unsigned long)[str retainCount]);
    [str retain];
    [str release];
    [str release];
    NSLog(@"%ld", (unsigned long)[str retainCount]);
    [ary removeAllObjects];
    NSLog(@"%ld", (unsigned long)[str retainCount]);

	解析:
		-1、-1、-1 。-1代表没有引用计数或者引用计数非常大，因为str是字符串，字符串在常量区，没有引用计数。引用计数为－1，这可以理解为NSString实际上是一个字符串常量，是没有引用计数的（或者它的引用计数是一个很大的值（使用%lu可以打印查看），对它做引用计数操作没实质上的影响）

	
	
	
参考资料:<br>
1.[OC面试系列1](https://www.jianshu.com/p/90c6b5151b7b)