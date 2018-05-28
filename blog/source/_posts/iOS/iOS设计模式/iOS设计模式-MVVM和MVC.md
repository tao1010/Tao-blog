---
title: iOS设计模式-MVVM和MVC
date: 2018-05-14 21:45:55
tags: OC
categories: iOS
---

一、MVC - Model View Controller		
1.MVC架构特点	

	尽量复用，保证重用;
		eg:
		一个抽象、封装的好的View，可以很便捷的移植到其他App;
		Model层的复用更多是一个产品多平台(iPhone、iPad)的复用;
	并不限制只有三层或三个类;
	
2.Controller中应该保留的代码：		

	不能复用的代码：
		初始化时，构造相应的View和Model;
		监听Model层的事件,将View层数的据传递到Model层;
		监听View层的事件,将View层的事件转发到Model层;
		eg:在一个包含TableView的Controller中
			将UITableView的data source分离到另外一个类中;
			将数据获取和转换的逻辑分离到另外一个类中;
			将封装控件的逻辑分离到另外一个类中;
	其他业务逻辑：
		将网络请求抽象到单独的类中;
			优点:
			 1.网络请求与具体的第三方库依赖分离,方便后期更换底层网络库;		
			 2.方便在基类处理公共逻辑;
			 3.方便在基类处理缓存逻辑;
			 4.方便做对象持久化;
		将界面的封装抽象到专门的类中;
			把类似于UILabel、UIButton、UItextField等用View封装起来;
		构造ViewModel：
			将Controller给View传递数据的过程抽象成ViewModel的过程；(View只接受ViewModel，Controller只传递ViewModel这一行代码)
			或
			将数据存取抽象到一个Service层,由该层提供ViewModel的获取；
		专门构造存储类：	
			数据的存储由专门的对象来做；
			eg:
				对一些热点数据增加缓存；
				处理数据迁移相关的逻辑；
3.Controller可拆分为：

	网络请求层；
	ViewModel层；
	Service层；
	Storage层；
	其他层；
二、MVVM - Model View ViewModel		
1.MVVM使用

	在使用中，通常还会利用双向绑定技术，使Model变化时，ViewModel会自动更新，当ViewModel变化时，View也会自动变化；
	在iOS中，KVO或Notification可以实现此效果；
2.MVVM第三方:[ReactiveCocoa](https://github.com/ReactiveCocoa/ReactiveCocoa)		

	函数式编程(Functional Programming)
	响应式编程(React Programming)
	无状态
	不可修改
3.MVVM作用
	
	实际使用中，能够使Model层和View层解藕；
4.MVVM问题
	
	数据绑定使bug很难被调试；
	对于过大项目，数据绑定需要花费更多的内存；









参考资料：		
1.[被误解的MVC和被神化的MVVM](http://blog.devtang.com/2015/11/02/mvc-and-mvvm/)		
2.[工厂模式](http://baike.baidu.com/view/1306799.htm)		


