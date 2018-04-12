---
title: iOS基础巩固-基础相关
date: 2018-04-09 21:31:04
tags: OC
categories: iOS
---

1.#import和#include的区别？#import&lt;&gt;和#import“”的区别？

	#import
		是Objective-C导入头文件的语法，确保引用的文件只会被引用一次，可保证不会重复导入;
		会链入该头文件的全部信息，包括实体变量和方法等;
	#include
		是C/C++导入头文件的语法，如果是OC与C/C++混编码，对于C/C++类型的文件，还是使用#include来引入，这种写法需要添加防重复导入的语法;
	@class
		前向声明，告诉编译器，其后面声明的名称是类的名称，至于这些类是如何定义的，暂时不用考虑。
		在头文件中，一般只需要知道被引用的类的名称就可以了。不需要知道其内部的实体变量和方法，
		在.h中一般使用@class来声明这个名称是类的名称。
		在.m里面，因为会用到这个引用类的内部的实体变量和方法，所以需要使用#import来包含这个被引用类的头文件;
<span>

	＃import<>引用系统文件，它用于对系统自带的头文件的引用，编译器会在系统文件目录下去查找该文件;
 	#import""用户自定义的文件用双引号引用，编译器首先会在用户目录下查找，然后到安装目录中查;
2.Object-C 有多继承吗?没有的话用什么代替?

	OC中是没有多继承的，但是可以通过“协议”和“分类‘来实现多重继承的效果
3.self和super的区别?

 	self调用自己方法，super调用父类方法;
  	self是类，super是预编译指令;
 	[self class]和[super class]输出是一样的;
 底层实现原理:
 
 	当使用self调用方法时，会从当前类的方法列表中开始找，如果没有，就从父类中再找;
 	当使用 self 调用时，会使用 objc_msgSend 函数： id objc_msgSend(id theReceiver, SEL theSelector, ...)。
 		第一个参数是消息接收者，
 		第二个参数是调用的具体类方法的 selector，后面是 selector 方法的可变参数。   
 	以 [self setName:] 为例，编译器会替换成调用 objc_msgSend 的函数调用，其中 theReceiver 是 self，theSelector 是 @selector(setName:)，这个 selector 是从当前 self 的 class 的方法列表开始找的 setName，当找到后把对应的 selector 传递过去。
<span>
 
  	当使用super时，则从父类的方法列表中开始找，然后调用父类的这个方法。
 	当使用 super 调用时，会使用 objc_msgSendSuper 函数：id objc_msgSendSuper(struct objc_super *super, SEL op, ...)
 		第一个参数是个objc_super的结构体，
 		第二个参数还是类似上面的类方法的selector，
		 struct objc_super {
			 id receiver;
			 Class superClass;
		 };
 	当编译器遇到  [super setName:] 时，开始做这几个事：
 		1）构 建 objc_super 的结构体，此时这个结构体的第一个成员变量 receiver 就是 子类，和 self 相同。而第二个成员变量 superClass 就是指父类调用 objc_msgSendSuper 的方法，将这个结构体和 setName 的 sel 传递过去。
 		2）函数里面在做的事情类似这样：从 objc_super 结构体指向的 superClass 的方法列表开始找 setName 的 selector，找到后再以 objc_super->receiver 去调用这个 selector.

4.NSMutableDictionary的setValueforkey和setObjectforkey的区别？与 KVC,setValueForKey 的用途?

| 语法 | Object | Value | key |
|---|:---:|:---:|:---:|
|setObject：forkey：|不能够为nil的，不然会报错| - |可以是任何类型|
|setValue： forKey：|-|value参数能够为nil，但是当value为nil的时候，会自动调用removeObject：forKey方法|只能够是NSString类型|
 	
注意：setObject：forKey：对象不能存放nil要与下面的这种情况区分：
	
	 1） [imageDictionarysetObject:[NSNullnull] forKey:indexNumber];
 	[NSNull null]表示的是一个空对象，并不是nil，注意这点
 
 	2） setObject：forKey：中Key是NSNumber对象的时候，如下：
		 [imageDictionarysetObject:obj forKey:[NSNumber numberWithInt：10]];
 注意：上面说的区别是针对调用者是dictionary而言的。
 	
 	setObject:forKey:方法NSMutabledictionary特有的,而
	setValue:forKey:方法是KVC（键-值编码）的主要方法。
 当 setValue:forKey:方法调用者是对象的时候：
 
 	setValue:forKey:方法是在NSObject对象中创建的，也就是说所有的oc对象都有这个方法，所以可以用于任何类。
 比如使用:
 	
 	SomeClass *someObj = [[SomeClass alloc] init];
 	[someObj setValue:self forKey:@"delegate"];
 	
 	表示的意思是：对象someObj设置他的delegate属性的值为当前类，当然调用此方法的对象必须要有delegate属性才能设置，不然调用了也没效果
 
objectForKey: 和 valueForKey: 在多数情况下都是一样的结果返回，但是如果 key 是以 @ 开头，valueForKey: 就成了一个大坑（去掉 key 里的 @ 然后用剩下部分作为 key 执行 [super valueForKey:]。去掉@在原有的key无法找到对应的值而crash）

建议在 NSDictionary 下只用 objectForKey: 来取值。	
5.为一个类添加方法的方式有哪些？

	1.定义该类方法；
	2.重写该类；
	3.继承该类；
	4.动态添加方法；
		 @interface EmptyClass:NSObject
		 @end
		 
		 @implementation EmptyClass
		 @end

		 		
		 { NSLog(@"Hello"); }
		 
		 - (void)addMethod
		 {
		     class_addMethod([EmptyClass class], @selector(sayHello2), (IMP)sayHello, "v@:");
		     // Test Method
		     EmptyClass *instance = [[EmptyClass alloc] init];
		     [instance sayHello2];
		     [instance release];
		 }
 	
 	
 	
 	
 	
 	
 	
 	
 	
	