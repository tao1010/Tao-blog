---
title: iOS多线程
date: 2018-03-27 15:12:08
tags: iOS
categories: iOS

---

一、多线程概述

1.基本概念

1.1 进程：在系统中正在运行的一个应用程序，每个进程均运行在其专用且受保护的内存空间内。

	是系统进行资源分配和调度的基本单位；
	是操作系统结构的基础，主要管理资源。
1.2 线程：一个进程(程序)的所有任务都在线程中执行，每个进程都至少有一个线程(主线程);
		
	线程是进程的基本执行单元，一个进程对应多个线程。
	
1.3 主线程：一个iOS程序运行后，默认会开启1条线程，称为“主线程”或“UI线程”；

	处理UI事件(eg:点击、滚动、拖拽事件等);
	显示或刷新UI界面 - 不要把耗时操作放在主线程，会卡界面。
1.4 多线程：一个进程中可以开启多条线程，多条线程可以并行(同时)执行不同的任务，多线程并发(同时)执行，其实是CPU快速地在多条线程之间调度(切换)

	在同一时刻，一个CPU只能处理1条线程，但CPU可以在多条线程之间快速的切换，只要切换的足够快，就造成了多线程一同执行的假象。
	多线程是通过提高资源使用率来提高系统总体的效率。  
	线程就像火车的一节车厢，进程则是火车。车厢（线程）离开火车（进程）是无法跑动的，而火车（进程）至少有一节车厢（主线程）。多线程可以看做多个车厢，它的出现是为了提高效率。	
2.生命周期   
![lifecircle](lifecircle.png)   
2.1 从图中可以看出线程的生命周期:新建-->就绪-->运行-->阻塞-->死亡;    
2.2 阐述：

	● 新建：实例化线程对象
	● 就绪：向线程对象发送start消息，线程对象被加入可调度线程池等待CPU调度。
    ● 运行：CPU 负责调度可调度线程池中线程的执行。线程执行完成之前，状态可能会在就绪和运行之间来回切换。就绪和运行之间的状态变化由CPU负责，程序员不能干预。
    ● 阻塞：当满足某个预定条件时，可以使用休眠或锁，阻塞线程执行。sleepForTimeInterval（休眠指定时长），sleepUntilDate（休眠到指定日期），@synchronized(self)：（互斥锁）。
    ● 死亡：正常死亡，线程执行完毕。非正常死亡，当满足某个条件后，在线程内部中止执行/在主线程中止线程对象
    ● 还有线程的exit和cancel
    ● [NSThread exit]：一旦强行终止线程，后续的所有代码都不会被执行。
    ● [thread cancel]取消：并不会直接取消线程，只是给线程对象添加 isCancelled 标记。
3.优缺点

||优点|缺点|
|---|---|---|
|多线程|1.能适当提高程序的执行效率;<br>2.能适当提高资源利用率(CPU、内存利用率)|1.创建线程是有开销的，iOS下主要包括:内核数据结构(大约1KB)、栈空间、创建时间90ms;<br>2.如果开启大量的线程，会减低程序的性能;<br>3.线程越多,CPU在调度线程上的开销就越大;<br>4.程序设计更加复杂:比如线程之间的通信,多线程的数据共享;|
|多进程|||
4.多线程安全



5.多线程的四种解决方案对比

|方案|简介|语言|线程生命周期|使用频率|
|---|---|---|---|---|
|pthread|1.一套通用的多线程API;<br>2.适用于Unix、Linux、Windows等系统;<br>3.跨平台、可移植;<br>4.使用难度大|C|程序员管理|几乎不用|
|NSThread|1.使用更加面向对象;<br>2.简单易用,可直接操作线程对象|OC|程序员管理|偶尔使用|
|GCD|1.旨在替代NSThread等线程技术<br>2.充分利用设备的多核|C|自动管理|经常使用|
|NSOperation|1.基于GCD(底层GCD)<br>2.比GCD多了一些更简单的实用功能<br>3.使用更加面向对象|OC|自动管理|经常使用|
二、NSThreadde使用   
![NSThread](NSThread.png)   
1.创建方式：

``` objectivec
//方法一：对象方法,需要手动开启
NSThread *thread1 = [[NSThread alloc] initWithTarget:self selector:@selector(startNSTreadAction:) object:@"NSThread_1"];
//线程加入线程池等待CPU调度，时间很快，几乎是立刻执行
[thread1 start];
    
//方法二：类方法，创建之后，自动开启
[NSThread detachNewThreadSelector:@selector(startNSTreadAction:) toTarget:self withObject:@"NSThread_2"];
    
//方法三：隐视创建，自动开启
[self performSelectorInBackground:@selector(startNSTreadAction:) withObject:@"NSThread_3"];

//MARK:Methods
- (void)startNSTreadAction:(NSObject *)object{
    
    NSLog(@"%@",object);
    NSLog(@"doSomething1：%@",[NSThread currentThread]);
}
```
2.类方法s s




参考资料：   
1.[多线程全套](https://www.jianshu.com/p/7649fad15cdb)  
2.[多线程和多进程的优缺点](https://www.cnblogs.com/Yogurshine/p/3640206.html)   
