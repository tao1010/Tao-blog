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

	
	
参考资料:<br>
1.[UI控件](https://www.jianshu.com/p/c2065cc6eb23)<br>
2.[传递和响应机制](https://www.jianshu.com/p/2e074db792ba)<br>
3.[Apple文档](https://developer.apple.com/documentation/uikit/touches_presses_and_gestures/understanding_event_handling_responders_and_the_responder_chain)