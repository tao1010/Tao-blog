---
title: iOS多线程
date: 2018-03-27 15:12:08
tags: iOS模块
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

4.多线程安全   
当多个线程访问同一块资源时,容易引发数据错乱和数据安全问题.解决方法：   
方法一：自旋锁atomic（效率最高）
	
	属性修饰atomic本身就有一把自旋锁：
	
	nonatomic 非原子属性,同一时间可以有很多线程读和写
	atomic 原子属性，保证同一时间只有一个线程能够写入(但是同一个时间多个线程都可以取值)，atomic 本身就有一把锁(自旋锁)
	
	nonatomic：非线程安全，不过效率更高，一般使用nonatomic
	atomic: 原子性，它仅限于getter,setter时的线程安全,所以对于一般数据类型如int，float它是可以保证线程安全的，对于NSString这种不可变字符串也能保证线程安全。对于NSMutable类型的属性，atmoic无法保证线程安全，对于这种属性往往都是对于数据块的读写操作，atmoic无法保证对于指针指向的数据库的线程安全。这种属性时使用nonatomic，自己处理线程安全。同样的对于NSMutable开头的一些类，使用atmoic还是会导致线程问题的

	解析：
	加了自旋锁，当新线程访问代码时，如果发现有其他线程正在锁定代码，新线程会用死循环的方式，一直等待锁定的代码执行完成。相当于不停尝试执行代码，比较消耗性能CPU资源
方法二：信号锁dispatch_semaphore是gcd中通过信号量来实现共享数据的数据安全。（效率第二）


方法三:互斥锁(同步锁 - pthread_mutex ，nslock ，synchronized)(效率最低)
	
	- (void)setName:(NSString *)name
	 { 
	     //互斥锁
	     @synchronized(self) 
	        { 
	              _name = [name copy]; 
	         } 
	}
    解析：
    若是在self对象上频繁加锁，那么程序可能要等另一段与此无关的代码执行完毕，才能继续执行当前代码，这样做其实并没有必要。
    判断的时候锁对象要存在，如果代码中只有一个地方需要加锁，大多都使用self作为锁对象，这样可以避免单独再创建一个锁对象。
	加了互斥做的代码，当新线程访问时，如果发现其他线程正在执行锁定的代码，新线程就会进入休眠
方法四：递归锁 pthread_mutex(recursive)与NSRecursiveLock ， 多次调用不会阻塞已获取该锁的线程。


方法五：条件锁 nsconditionlock 满足一定的条件的加锁和解锁，可以实现依赖关系。nscondition条件锁，也是通过信号量来解锁，主要用来实现生产者消费者模式。

5.多线程的四种解决方案对比

|方案|简介|语言|线程生命周期|使用频率|
|---|---|---|---|---|
|pthread|1.一套通用的多线程API;<br>2.适用于Unix、Linux、Windows等系统;<br>3.跨平台、可移植;<br>4.使用难度大|C|程序员管理|几乎不用|
|NSThread|1.使用更加面向对象;<br>2.简单易用,可直接操作线程对象|OC|程序员管理|偶尔使用|
|GCD|1.旨在替代NSThread等线程技术<br>2.充分利用设备的多核|C|自动管理|经常使用|
|NSOperation|1.基于GCD(底层GCD)<br>2.比GCD多了一些更简单的实用功能<br>3.使用更加面向对象|OC|自动管理|经常使用|
二、NSThread的使用
   
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
2.类方法

	//当前线程 打印结果为：{number = 1,name = main} 表示主线程，其他表示子线程
    [NSThread currentThread];
    
    //阻塞主线程
    [NSThread sleepForTimeInterval:10.0];//时间
    [NSThread sleepUntilDate:[NSDate date]];//日期
    //退出线程
    [NSThread exit];
    //是否为主线程
    [NSThread isMainThread];
    //是否是多线程
    [NSThread isMultiThreaded];
    
3.属性
	
	//主线程对象
    NSThread *thread = [NSThread mainThread];
    //线程是否在执行
    thread.isExecuting;
    //线程是否被取消
    thread.isCancelled;
    //线程是否完成
    thread.isFinished;
    //线程是否主线程
    thread.isMainThread;
    //线程的优先级，取值范围0.0到1.0，默认优先级0.5，1.0表示最高优先级，优先级高，CPU调度的频率高
    thread.threadPriority;
三、GCD的使用    
![GCD](GCD.png)   
1.基本概念       

- 任务（block）：任务就是将要在线程中执行的代码，将这段代码用block封装好，然后将这个任务添加到指定的执行方式（同步执行和异步执行），等待CPU从队列中取出任务放到对应的线程中执行。
-  同步（sync）：一个接着一个，前一个没有执行完，后面不能执行，不开线程。
- 异步（async）：开启多个新线程，任务同一时间可以一起执行。异步是多线程的代名词
- 队列：装载线程任务的队形结构。(系统以先进先出的方式调度队列中的任务执行)。在GCD中有两种队列：串行队列和并发队列。
- 并发队列：线程可以同时一起进行执行。实际上是CPU在多条线程之间快速的切换。（并发功能只有在异步（dispatch_async）函数下才有效）
- 串行队列：线程只能依次有序的执行。
- GCD总结：将任务(要在线程中执行的操作block)添加到队列(自己创建或使用全局并发队列)，并且指定执行任务的方式(异步dispatch_async，同步dispatch_sync)    
2.特点

- GCD会自动利用更多的CPU内核
- GCD自动管理线程的生命周期（创建线程，调度任务，销毁线程等）
- 程序员只需要告诉 GCD 想要如何执行什么任务，不需要编写任何线程管理代码

3.队列创建方法

方法一：串行队列|并行队列dispatch_queue_create(@"xx",x);

	dispatch_queue_create:
	// 串行队列 - FIFO
	dispatch_queue_t queue = dispatch_queue_create("test", DISPATCH_QUEUE_SERIAL);
	// 并发队列
	dispatch_queue_t queue1 = dispatch_queue_create("test", DISPATCH_QUEUE_CONCURRENT);
	解析:
	使用dispatch_queue_create来创建队列对象，传入两个参数:
	第一个参数表示队列的唯一标识符，可为空。
	第二个参数用来表示串行队列（DISPATCH_QUEUE_SERIAL）或
					  并发队列（DISPATCH_QUEUE_CONCURRENT);
方法二：主队列dispatch_get_main_queue()

	dispatch_async(dispatch_get_global_queue(0, 0), ^{
	 	 // 耗时操作放在这
	 	 
		 dispatch_async(dispatch_get_main_queue(), ^{
		 	// 回到主线程进行UI操作
		 	
		 });
	 });
	 解析:
	 主队列负责在主线程上调度任务，如果在主线程上已经有任务正在执行，主队列会等到主线程空闲后再调度任务。通常是返回主线程更新UI的时候使用。
	 
方法三：全局并发队列 dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT,0);

	dispatch_queue_t queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);
	//全局并发队列的优先级
	#define DISPATCH_QUEUE_PRIORITY_HIGH 2 // 高优先级
	#define DISPATCH_QUEUE_PRIORITY_DEFAULT 0 // 默认（中）优先级
	#define DISPATCH_QUEUE_PRIORITY_LOW (-2) // 低优先级
	#define DISPATCH_QUEUE_PRIORITY_BACKGROUND INT16_MIN // 后台优先级
	//iOS8开始使用服务质量，现在获取全局并发队列时，可以直接传0
	dispatch_get_global_queue(0, 0);
	解析：
	全局并发队列是就是一个并发队列，是为了让我们更方便的使用多线程。dispatch_get_global_queue(0, 0)
4.同步|异步|任务 - 创建方式    
同步（sync）使用dispatch_sync来表示。    
异步（async）使用dispatch_async。    
任务就是将要在线程中执行的代码，将这段代码用block封装好。    

	    // 同步执行任务
    dispatch_sync(dispatch_get_global_queue(0, 0), ^{
        // 任务放在这个block里
        NSLog(@"我是同步执行的任务");
        
    });
    // 异步执行任务
    dispatch_async(dispatch_get_global_queue(0, 0), ^{
        // 任务放在这个block里
        NSLog(@"我是异步执行的任务");
        
    });
5.多种组合方式:队列(串行|并发|主队列) + 执行方式(同步|异步)     

串行同步：主线程顺序执行    
 
    // 串行队列
    dispatch_queue_t queue = dispatch_queue_create("test", DISPATCH_QUEUE_SERIAL);
    
    // 同步执行
    dispatch_sync(queue, ^{
        for (int i = 0; i < 3; i++) {
            NSLog(@"串行同步1 %@",[NSThread currentThread]);
        }
    });
    dispatch_sync(queue, ^{
        for (int i = 0; i < 3; i++) {
            NSLog(@"串行同步2 %@",[NSThread currentThread]);
        }
    });
    dispatch_sync(queue, ^{
        for (int i = 0; i < 3; i++) {
            NSLog(@"串行同步3 %@",[NSThread currentThread]);
        }
    });
	解析：执行完一个任务，再执行下一个任务。不开启新线程，打印结果验证为顺序在主线程执行。
串行异步：多线程顺序执行
	
	// 串行队列
    dispatch_queue_t queue = dispatch_queue_create("test", DISPATCH_QUEUE_SERIAL);
    
    // 异步执行
    dispatch_async(queue, ^{
        for (int i = 0; i < 3; i++) {
            NSLog(@"串行异步1 %@",[NSThread currentThread]);
        }
    });
    dispatch_async(queue, ^{
        for (int i = 0; i < 3; i++) {
            NSLog(@"串行异步2 %@",[NSThread currentThread]);
        }
    });
    dispatch_async(queue, ^{
        for (int i = 0; i < 3; i++) {
            NSLog(@"串行异步3 %@",[NSThread currentThread]);
        }
    });
	解析：开启新线程，顺序执行任务
并发同步：主线程顺序执行

	 // 并发队列
	 dispatch_queue_t queue = dispatch_queue_create("test", DISPATCH_QUEUE_CONCURRENT);
	 
	 // 同步执行
	 dispatch_sync(queue, ^{
		 for (int i = 0; i < 3; i++) {
			 NSLog(@"并发同步1 %@",[NSThread currentThread]);
		 }
	 });
	 dispatch_sync(queue, ^{
		 for (int i = 0; i < 3; i++) {
			 NSLog(@"并发同步2 %@",[NSThread currentThread]);
		 }
	 });
	 dispatch_sync(queue, ^{
		 for (int i = 0; i < 3; i++) {
			 NSLog(@"并发同步3 %@",[NSThread currentThread]);
		 }
	 });
	解析：主线程，顺序执行
并发异步：多线程交替无序执行

	// 并发队列
    dispatch_queue_t queue = dispatch_queue_create("test", DISPATCH_QUEUE_CONCURRENT);
    
    // 异步执行
    dispatch_async(queue, ^{
        for (int i = 0; i < 3; i++) {
            NSLog(@"并发异步1 %@",[NSThread currentThread]);
        }
    });
    dispatch_async(queue, ^{
        for (int i = 0; i < 3; i++) {
            NSLog(@"并发异步2 %@",[NSThread currentThread]);
        }
    });
    dispatch_async(queue, ^{
        for (int i = 0; i < 3; i++) {
            NSLog(@"并发异步3 %@",[NSThread currentThread]);
        }
    });
	解析：开启多线程，任务交替进行，无序执行
主队列同步：主线程中运行，发生死锁，crash

	// 主队列
    dispatch_queue_t queue = dispatch_get_main_queue();
    
    dispatch_sync(queue, ^{
        for (int i = 0; i < 3; i++) {
            NSLog(@"主队列同步1 %@",[NSThread currentThread]);
        }
    });
    dispatch_sync(queue, ^{
        for (int i = 0; i < 3; i++) {
            NSLog(@"主队列同步2 %@",[NSThread currentThread]);
        }
    });
    dispatch_sync(queue, ^{
        for (int i = 0; i < 3; i++) {
            NSLog(@"主队列同步3 %@",[NSThread currentThread]);
        }
    });
	解析：主队列同步死锁原因:
	1. 如果在主线程中运用主队列同步，也就是把任务放到了主线程的队列中。
	2. 而同步对于任务是立刻执行的，那么当把第一个任务放进主队列时，它就会立马执行。
	3. 可是主线程现在正在处理syncMain方法，任务需要等syncMain执行完才能执行。
	4. syncMain执行到第一个任务的时候，又要等第一个任务执行完才能往下执行第二个和第三个任务。
	5. 这样syncMain方法和第一个任务就开始了互相等待，形成了死锁。
主队列异步：主线程顺序执行

	// 主队列
    dispatch_queue_t queue = dispatch_get_main_queue();
    
    dispatch_async(queue, ^{
        for (int i = 0; i < 3; i++) {
            NSLog(@"主队列异步1 %@",[NSThread currentThread]);
        }
    });
    dispatch_async(queue, ^{
        for (int i = 0; i < 3; i++) {
            NSLog(@"主队列异步2 %@",[NSThread currentThread]);
        }
    });
    dispatch_async(queue, ^{
        for (int i = 0; i < 3; i++) {
            NSLog(@"主队列异步3 %@",[NSThread currentThread]);
        }
    });
6.GCD线程间通讯

eg1:图片下载刷新

	子线程下载图片，完成后主线程刷新UI
	 // 异步
    dispatch_async(dispatch_get_global_queue(0, 0), ^{
        // 耗时操作放在这里，例如下载图片。（运用线程休眠两秒来模拟耗时操作）
        [NSThread sleepForTimeInterval:2];
        NSURL *picURL = [NSURL URLWithString:@"http://www.bangmangxuan.net/xxx.jpg"];
        NSData *picData = [NSData dataWithContentsOfURL:picURL];
        UIImage *image = [UIImage imageWithData:picData];
        
        // 回到主线程处理UI
        dispatch_async(dispatch_get_main_queue(), ^{
            // 在主线程上添加图片
            self.imageView.image = image;
        });
    });
eg2:下载视频和音频，必须是视频下载完成后再下载音频 - GCD栅栏dispatch_barrier_async|dispatch_barrier_sync   
![dispatch_barrier](dispatch_barrier.png) 
  
||dispatch_barrier_sync|dispatch_barrier_async|
|---|---|---|
|相同|1.都会等待在它前面插入队列的任务（1、2、3）先执行完 <br>2.都会等待他们自己的任务（0）执行完再执行后面的任务（4、5、6）||
|不同|在将任务插入到queue的时候，dispatch_barrier_sync需要等待自己的任务（0）结束之后才会继续程序，然后插入被写在它后面的任务（4、5、6），然后执行后面的任务|dispatch_barrier_async将自己的任务（0）插入到queue之后，不会等待自己的任务结束，它会继续把后面的任务（4、5、6）插入到queue|
所以，dispatch_barrier_async的不等待（异步）特性体现在将任务插入队列的过程，它的等待特性体现在任务真正执行的过程

	// 并发队列
    dispatch_queue_t queue = dispatch_queue_create("test", DISPATCH_QUEUE_CONCURRENT);
    
    // 异步执行
    dispatch_async(queue, ^{
        for (int i = 0; i < 3; i++) {
            NSLog(@"栅栏：并发异步1 %@",[NSThread currentThread]);
        }
    });
    dispatch_async(queue, ^{
        for (int i = 0; i < 3; i++) {
            NSLog(@"栅栏：并发异步2 %@",[NSThread currentThread]);
        }
    });
    
    dispatch_barrier_async(queue, ^{
        NSLog(@"------------barrier------------%@", [NSThread currentThread]);
        NSLog(@"******* 并发异步执行，但是34一定在12后面 *********");
    });
    
    dispatch_async(queue, ^{
        for (int i = 0; i < 3; i++) {
            NSLog(@"栅栏：并发异步3 %@",[NSThread currentThread]);
        }
    });
    dispatch_async(queue, ^{
        for (int i = 0; i < 3; i++) {
            NSLog(@"栅栏：并发异步4 %@",[NSThread currentThread]);
        }
    });
	解析：开启多条线程，所有任务并发异步进行
	适用场景：当任务需要异步进行,但是这些任务需要分多步执行，第一组完成后才能进行第二组操作。
eg3:延时dispatch_after

	dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(5.0 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
        // 5秒后异步执行
        NSLog(@"我已经等待了5秒！");
    });
eg4:只执行一次dispatch_once

    //使用dispatch_once能保证某段代码在程序运行过程中只被执行1次。可以用来设计单例。

    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        NSLog(@"程序运行过程中我只执行了一次！");
    });
eg5:快速迭代dispatch_apply
	
	// 并发队列
    dispatch_queue_t queue = dispatch_get_global_queue(0, 0);
    
    // dispatch_apply几乎同时遍历多个数字
    dispatch_apply(7, queue, ^(size_t index) {
    
        NSLog(@"dispatch_apply：%zd======%@",index, [NSThread currentThread]);
    });
eg6:GCD队列组
  
    dispatch_group_t group = dispatch_group_create();
    
    dispatch_group_async(group, dispatch_get_global_queue(0, 0), ^{
        NSLog(@"队列组：有一个耗时操作完成！");
    });
    
    dispatch_group_async(group, dispatch_get_global_queue(0, 0), ^{
        NSLog(@"队列组：有一个耗时操作完成！");
    });
    
    dispatch_group_notify(group, dispatch_get_main_queue(), ^{
        NSLog(@"队列组：前面的耗时操作都完成了，回到主线程进行相关操作");
    });
    解析:异步执行几个耗时操作，当这几个操作都完成之后再回到主线程进行操作，就可以用到队列组了。
    
    队列组有下面几个特点：
     1.所有的任务会并发的执行(不按序)。
     2.所有的异步函数都添加到队列中，然后再纳入队列组的监听范围。
     3.使用dispatch_group_notify函数，来监听上面的任务是否完成，如果完成, 就会调用这个方法。
四、NSOperation的使用     
<!--![Operation](Operation.png)-->
![NSOperation](NSOperation.png)
NSOperation是基于GCD之上的更高一层的封装,需要配合NSOperationQueue来实现多线程。

1.NSOperation实现步骤:
	
	1)创建任务：先将需要执行的操作封装到NSOperation对象中；
    2)创建队列：创建NSOperationQueue；
    3)将任务加入到队列中：将NSOperation对象添加到NSOperationQueue中；
2.NSOperation的三种创建方式:(NSOperation是个抽象类，运用时需要使用其子类)

方式一:NSInvocationOperation; 
	
	 NSInvocationOperation *invocationOperation = [[NSInvocationOperation alloc] initWithTarget:self selector:@selector(invocationOperationAction:) object:@"helloworld"];
    [invocationOperation start];

    //Method
    - (void)invocationOperationAction:(NSObject *)object{
	    NSLog(@"包含的任务，没有加入队列====%@====%@",object,[NSThread currentThread]);
	}
	解析：程序在主线程中执行，没有开启新线程，因为需要配合NSOperationQueue使用;
方式二:NSBlockOperation;

不加入子线程方法：

	 NSBlockOperation *blockOperation = [NSBlockOperation blockOperationWithBlock:^{
        NSLog(@"NSBlockOperation包含的任务，没有加入队列====%@",[NSThread currentThread]);
    }];
    [blockOperation start];
    解析：程序在主线程中执行，没有开启新线程，因为需要配合NSOperationQueue使用;
加入子线程方法：
 
    NSBlockOperation *blockOperation = [[NSBlockOperation alloc] init];
    [blockOperation addExecutionBlock:^{
        NSLog(@"NSBlockOperation运用addExecutionBlock方法添加任务1========%@", [NSThread currentThread]);
    }];
    [blockOperation addExecutionBlock:^{
        NSLog(@"NSBlockOperation运用addExecutionBlock方法添加任务2========%@", [NSThread currentThread]);
    }];
    [blockOperation addExecutionBlock:^{
        NSLog(@"NSBlockOperation运用addExecutionBlock方法添加任务3========%@", [NSThread currentThread]);
    }];
    [blockOperation start];
    解析：添加addExecutionBlock：可以使NSBlockOperation实现多线程
方式三:定义继承自NSOperation的子类,通过实现内部相应的方法来封装任务;
	
定义一个继承NSOperation的类：（重写main方法，之后就可以使用这个子类进行相关操作）
	
	DTOperation.h
	#import <Foundation/Foundation.h>

	@interface DTOperation : NSOperation
	
	@end
	---------------------------------------
	DTOperation.m
	#import "DTOperation.h"

	@implementation DTOperation
	
	- (void)main {
		 for (int i = 0; i < 3; i++) {
			 NSLog(@"NSOperation的子类WHOperation======%@",[NSThread currentThread]);
		 }
	}
	@end
使用方式（在需要的控制器中）

	#import "DTOperation.h"
	
	DTHomeController.m
		
	DTOperation *operation = [[DTOperation alloc] init];
	[operation start];
	
	解析：添加addExecutionBlock：可以使NSBlockOperation实现多线程
3.队列NSOperationQueue:主队列+其他队列(串行队列+并行队列)

	//主队列
	NSOperationQueue *mainQueue = [NSOperationQueue mainQueue];
	
	//其他队列
	NSOperationQueue *queue = [[NSOperationQueue alloc] init];
最大并发数: maxConcurrentOperationCount

	 默认为-1，直接并发执行，所以加入到‘非队列’中的任务默认就是并发，开启多线程。
    当maxConcurrentOperationCount为1时，则表示不开线程，也就是串行。
    当maxConcurrentOperationCount大于1时，进行并发执行。
    系统对最大并发数有一个限制，所以即使程序员把maxConcurrentOperationCount设置的很大，系统也会自动调整。所以把最大并发数设置的很大是没有意义的。
4.NSOperation的常规使用方式：把任务加入队列（NSOperation + NSOperationQueue） 

addOperation添加任务到队列
	
	// 创建队列，默认并发
    NSOperationQueue *queue = [[NSOperationQueue alloc] init];
    
    // 创建操作，NSInvocationOperation
    NSInvocationOperation *invocationOperation = [[NSInvocationOperation alloc] initWithTarget:self selector:@selector(invocationOperationAddOperation) object:nil];
    
    // 创建操作，NSBlockOperation
    NSBlockOperation *blockOperation = [NSBlockOperation blockOperationWithBlock:^{
    
        for (int i = 0; i < 3; i++) {
            NSLog(@"addOperation把任务添加到队列======%@", [NSThread currentThread]);
        }
    }];
    [queue addOperation:invocationOperation];
    [queue addOperation:blockOperation];

	//Method
	- (void)invocationOperationAddOperation {
	    NSLog(@"===aaddOperation把任务添加到队列====%@", [NSThread currentThread]);
	}
	解析：开启了新线程
addOperationWithBlock添加任务到队列
	
	 // 创建队列，默认并发
    NSOperationQueue *queue = [[NSOperationQueue alloc] init];
    
    // 添加操作到队列
    [queue addOperationWithBlock:^{
        for (int i = 0; i < 3; i++) {
            NSLog(@"addOperationWithBlock把任务添加到队列======%@", [NSThread currentThread]);
        }
    }];

运用最大并发数实现串行
    
    // 创建队列，默认并发
	 NSOperationQueue *queue = [[NSOperationQueue alloc] init];
	 
	 // 最大并发数为1，串行
	 queue.maxConcurrentOperationCount = 1;
	 
	 // 最大并发数为2，并发
	 // queue.maxConcurrentOperationCount = 2;
	
	 // 添加操作到队列
	 [queue addOperationWithBlock:^{
		 for (int i = 0; i < 3; i++) {
			 NSLog(@"addOperationWithBlock把任务添加到队列1======%@", [NSThread currentThread]);
		 }
	 }];
	 // 添加操作到队列
	 [queue addOperationWithBlock:^{
		 for (int i = 0; i < 3; i++) {
			 NSLog(@"addOperationWithBlock把任务添加到队列2======%@", [NSThread currentThread]);
		 }
	 }];
	 
	 // 添加操作到队列
	 [queue addOperationWithBlock:^{
		 for (int i = 0; i < 3; i++) {
			 NSLog(@"addOperationWithBlock把任务添加到队列3======%@", [NSThread currentThread]);
		 }
	 }];
    解析:
    默认maxConcurrentOperationCount = -1，启了线程，无序执行，并发。
    maxConcurrentOperationCount = 1时，开启了线程，顺序执行，串行。
    maxConcurrentOperationCount = 2时，开启了线程，无序执行，并发。
5.NSOperation其他操作

对象方法：
	
	//取消队列NSOperationQueue所有操作
	- (void)cancelAllOperations;
	
	//取消NSOperation某个操作
	- (void)cancel
	
	//完成后的回调方法
	operation.completionBlock = <#^(void)#>
	
	//使队列暂停    
    [queue setSuspended:YES];
   
    //判断队列是否暂停
    - (BOOL)isSuspended
    
    暂停和取消不是立刻取消当前操作，而是等当前的操作执行完之后不再进行新的操作；
6.NSOperation的操作依赖
  
使用场景:某一个操作（operation2）依赖于另一个操作（operation1），只有当operation1执行完毕，才能执行operation2;

	// 并发队列
    NSOperationQueue *queue = [[NSOperationQueue alloc] init];
    
    // 操作1
    NSBlockOperation *operation1 = [NSBlockOperation blockOperationWithBlock:^{
        for (int i = 0; i < 3; i++) {
            NSLog(@"operation1======%@", [NSThread currentThread]);
        }
    }];
    
    // 操作2
    NSBlockOperation *operation2 = [NSBlockOperation blockOperationWithBlock:^{
    
        NSLog(@"****operation2依赖于operation1，只有当operation1执行完毕，operation2才会执行****");
        for (int i = 0; i < 3; i++) {
            NSLog(@"operation2======%@", [NSThread currentThread]);
        }
    }];
    
    // 使操作2依赖于操作1
    [operation2 addDependency:operation1];
    // 把操作加入队列
    [queue addOperation:operation1];
    [queue addOperation:operation2];
    
参考资料：   
1.[多线程全套](https://www.jianshu.com/p/7649fad15cdb)  
2.[多线程和多进程的优缺点](https://www.cnblogs.com/Yogurshine/p/3640206.html)   
3.[多线程安全](https://www.jianshu.com/p/27af7fae9947)