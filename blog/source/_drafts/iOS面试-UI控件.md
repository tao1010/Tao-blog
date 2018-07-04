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

	事件的传递顺序:
		产生触摸事件->UIApplication事件队列->[UIWindow hitTest:withEvent:]->返回更合适的view->[子控件 hitTest:withEvent:]->返回最合适的view
		
	事件的生命周期
		1>事件的产生
			触摸发生后，系统将该事件 加入一个由 UIApplication 管理的事件队列(队列：FIFO,栈:FILO)中;
			UIApplication 会从事件队列中取出最前面的事件，并将事件分发下去以便处理，通常先发送事件给应用程序的主窗口keyWindow;
			主窗口会在视图层次结构中找到最合适的视图来处理触摸事件，找到合适的视图控件后，会调用视图控件的touches 方法来作具体的事件处理。
		2>事件的传递
			
			*重点* 事件如何从父控件传递到子控件并寻找到最合适的view
				1.主窗口接收到应用程序传递过来的事件后，判断(自己能否接受触摸事件;若能，继续判断触摸点是否在自己身上)
				2.触摸点在自己身上，遍历子控件(从后往前)
				3.在每个子控件上重复判断步骤(1.判断子控件能否接受事件，2.点在不在子控件上)
				4.直到找到最合适的View(能接受事件，点在自己身上)
				
			*难点* 寻找最合适的view的底层实现 hitTest:withEvent 底层实现
			拦截事件的处理
				
				hitTest：withEvent：方法
					调用时间:只要事件一传递给一个控件，就会调用
					作用:寻找并返回最合适的view(能够响应事件的view)
					拦截处理:重写此方法，返回需要指定的view即可
					返回nil时，就表示父控件是最合适的view
			触摸事件的传递是从父控件 传递到子控件；
			UIApplication -->Window -->寻找处理事件最合适的view
		
		3>事件的处理(找到最合适的view后)
			5.调用touches 方法具体事件
		
	UIResponder - 响应者对象
		UIApplication
		UIViewController
		UIView
	UIResponder - 事件处理
		触摸
		 	touchesBegan
		 		若2根手指同时触摸，调用一次，touches参数中有2个UITouch对象
		 		若2根手指一前一后触摸，调用2次，每次调用touches参数只有一个UITouch对象
		 	touchesMoved
		 	touchesEnded
		 	touchesCancelled
		加速计事件 - 不遵循响应链
			motionBegan
			motionEnded
			motionCancelled
		远程控制事件 - 耳机
			remoteControlReceived

	UITouch作用
		保存跟手指相关的信息,比如触摸位置、时间、阶段；
		当手指移动时，系统会更新同一个UITouch对象，使之能够一直保存该手指在的触摸位置；
		当手指离开屏幕时，系统会销毁响应的UITouch对象；
7.事件响应
	
	响应者：
		能处理事件的对象，即继承自UIResponder的对象。
	响应者链条：
		由多个响应者对象连接起来的链条。
	作用: 
		很清楚的看见每个响应者之间的联系，并且可以让一个事件多个对象处理。
	响应者链的事件传递过程：
		如果当前view是控制器的view，那么控制器就是上一个响应者，事件就传递给控制器；如果当前view不是控制器的view，那么父视图就是当前view的上一个响应者，事件就传递给它的父视图
		在视图层次结构的最顶级视图，如果也不能处理收到的事件或消息，则其将事件或消息传递给window对象进行处理
		如果window对象也不处理，则其将事件或消息传递给UIApplication对象
		如果UIApplication也不能处理该事件或消息，则将其丢弃

	
参考资料:<br>
1.[UI控件](https://www.jianshu.com/p/c2065cc6eb23)<br>
2.[传递和响应机制](https://www.jianshu.com/p/2e074db792ba)<br>
3.[Apple文档](https://developer.apple.com/documentation/uikit/touches_presses_and_gestures/understanding_event_handling_responders_and_the_responder_chain)