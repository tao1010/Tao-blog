---
title: iOS面试-UI控件
toc: true
date: 2018-07-03 23:16:58
tags: iOS面试
categories: iOS
---

1.怎么解决缓存池满的问题(cell)

<!-- more -->

	ios中不存在缓存池满的情况，因为通常我们ios中开发，对象都是在需要的时候才会创建，有种常用的说话叫做懒加载，还有在UITableView中一般只会创建刚开始出现在屏幕中的cell，之后都是从缓存池里取，不会在创建新对象;
	缓存池里最多也就一两个对象，缓存池满的这种情况一般在开发java中比较常见，java中一般把最近最少使用的对象先释放;
2.CAAnimation的层级结构


3.UIButton与UITableView的层级结构

	继承结构
	内部的子控件结构
4.如何渲染自定义格式字符的UILabel
	
	通过NSAttributedString类
5.设置UIScrollView 的 contentsize 能在 viewdidLoad 中吗，为什么？
	
	能
6.UIButton或其他UIView控件的事件传递过程

	触摸事件的传递： 
		父控件 --> 子控件 详情如下：
		UIApplication --> UIWindow --> 处理事件最合适的View
	寻找最合适的控件：
		1.判断keyWindow(主窗口)自己是否能接受触摸事件;
		2.判断触摸点是否在自己身上;
		3.遍历子控件数组(从后往前 栈 - 最后入栈的控件开始),对每个控件进行上述1，2步骤的判断;
		4.找到合适控件,将事件交给该控件处理，再遍历该控件的子控件,直达没有合适的控件为止;
	UIView不能接受控件的三种情况:
		userInteractionEnabled = NO
		hidden = YES
		alpha < 0.01
7.事件响应流程

	1.一个UIView发出一个事件之后,上传给父视图;
	2.父视图上传给所在的控制器;
	3.如果控制器对事件进行处理,事件传递终止,否则继续上传父视图;
	4.直到遇到响应者才停止,否则事件一直上传,直到UIWindow。

8.控制器View的生命周期和相关函数
	
	1.判断控制器是否有View,没有则用loadView方法创建:SB或代码;
	2.调用:viewDidLoad 初始化操作 -- 只被调用一次;
	3.view显示之前 调用:viewWillAppear --可多次调用;
	4.调用:viewDidAppear;
	5.view消失之前 调用:viewWillDisappear --可多次调用;
	6.布局变化前后 调用:viewWill/DidLayoutSubviews处理相关信息;
9.UIScrollView用到的设计模式,在Foundation库中是否有类似的模式
	
	模版模式 - 所有 datasource和delegate 都是模版模式的典型应用;
	组合模式 - 所有 containerView 都是这个模式;
	观察者模式 - 所有 UIResponder 都用这个模式;
10.动态绑定 - 在runtime 确定调用的方法
	
	动态绑定将调用方法的确定也推迟到运行时。
	在编译时，方法的调用并不和代码绑定在一起，只有在消息发送出来之后，才确定被调用的代码。
	通过动态类型和动态绑定技术，代码每次执行都可以得到不同的结果。
	运行时因子负责确定消息的接收者和被调用的方法。
	运行时的消息分发机制为动态绑定提供支持。
	当向一个动态类型确定了的对象发送消息时，运行环境系统会通过接收者的 isa 指针定位对象的类，并以此为起点确定被调用的方法，方法和消息是动态绑定的。
	不必在 Objective-C 代码中做任何工作，就可以自动获取动态绑定的好处。

参考资料:<br>
1.[UI控件](https://www.jianshu.com/p/c2065cc6eb23)<br>
2.[传递和响应机制](https://www.jianshu.com/p/2e074db792ba)<br>
3.[Apple文档](https://developer.apple.com/documentation/uikit/touches_presses_and_gestures/understanding_event_handling_responders_and_the_responder_chain)    
4.[](https://zhuanlan.zhihu.com/c_154646059)