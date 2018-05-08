---
title: iOS第三方库-SDWebImage
date: 2018-05-07 14:23:54
tags: OC
categories: iOS
---

一、内存缓存和磁盘缓存区别		
1.缓存		
	
	分类：内存缓存和磁盘(硬盘)缓存；
	内存：
		指当前程序的运行空间，缓存速度快容量小，是临时存储文件用的，供CPU直接读取，比如说打开一个程序,他是在内存中存储,关闭程序后内存就又回到原来的空闲空间；
	磁盘：
		程序的存储空间，缓存容量大速度慢可持久化与内存不同的是磁盘是永久存储东西的，只要里面存放东西,不管运行不运行 ，他都占用空间！磁盘缓存是存在Library/Caches；
2.内存		
![sdw1](sdw1.png)		
	
	iOS内存分5个区：栈区、堆区、全局区、常量区、代码区；
	栈区(stack)：这一块区域系统会自己管理，我们不用干预，主要存一些局部变量，以及函数跳转时的现场保护。因此大量的局部变量,深递归，函数循环调用都可能导致内存耗尽而运行崩溃;
	堆区(heap)：与栈区相对，这一块一般由我们自己管理，比如alloc，free的操作，存储一些自己创建的对象;
	全局区(静态区static)：全局变量和静态变量都存储在这里，已经初始化的和没有初始化的会分开存储在相邻的区域，程序结束后系统会释放;
	常量区：存储常量字符串和const常量;
	代码区：存储代码;
3.磁盘(硬盘)		
![sdw2](sdw2.png)		
	
	iOS沙盒机制：
		iOS应用程序只能在该程序创建的文件系统中读取文件，不可以去其它地方访问，此区域被成为沙盒，所以所有的非代码文件都要保存在此，例如图像，图标，声音，映像，属性列表，文本文件等;
	沙盒：默认情况下，每个沙盒含有3个文件夹：Documents, Library 和 tmp;
		 Documents：苹果建议将程序中建立的或在程序中浏览到的文件数据保存在该目录下，iTunes备份和恢复的时候会包括此目录;
        Library：存储程序的默认设置或其它状态信息；
        		Library/Caches：存放缓存文件，iTunes不会备份此目录，此目录下文件不会在应用退出删除.
       		Library/preferences: 存放的是 user default 存储的信息，iTunes会备份此目录， 应用程序重新启动不会丢弃数据，我们使用 NSUserDefaults写的设置数据都会保存到该目录下的一个plist文件中，这就是所谓的写到plist中！
        tmp：提供一个即时创建临时文件的地方， iTunes不会备份此目录;
    
    用户生成的文件放在documents，自己的文件放在library/cache里面，简单的说明：
    如果你做个记事本的app，那么用户写了东西，总要把东西存起来。那么这个文件则是用户自行生成的，就放在documents文件夹里面。
    如果你有一个app，需要和服务器配合，经常从服务器下载东西，展示给用户看。那么这些下载下来的东西就放在library/cache。
    apple对这个很严格，放错了就会被拒。主要原因是ios的icloud的同步问题。
二、SDWebImage基础		
1.版本信息:pod 'SDWebImage','~>4.3'
2.结构图:
![sdw3](sdw3.png)			
3.功能描述:异步图像下载并且带有缓存机制,是UIImageView的一个分类;     
4.框架特征:
	
	类别UIImageView，UIButton，MKAnnotationView- - 添加Web图像和高速缓存管理;
	异步图像下载器;
	具有自动缓存、到期处理的异步内存+磁盘映像缓存;
	背景图像解压缩;
	保证相同的URL不会被下载多次;
	保证虚假网址不会重复重试;
	保证主线程永远不会被阻止;
	使用GCD和ARC;
5.支持的图像格式

	UIImage（JPEG，PNG，...）支持的图像格式，包括gif;
	WebP格式，包括动画WebP（使用WebPsuspec）;
6.使用方法:		
Objective-C:			

	#import <SDWebImage/UIImageView+WebCache.h>
	...
	[imageView sd_setImageWithURL:[NSURL URLWithString:@"http://www.domain.com/path/to/image.jpg"] placeholderImage:[UIImage imageNamed:@"placeholder.png"]];
Swift:
	
	import SDWebImage

	imageView.sd_setImage(with: URL(string: "http://www.domain.com/path/to/image.jpg"), placeholderImage: UIImage(named: "placeholder.png"));
三、SDWebImage原理		
1.导入类：import  "UIImageView+WebCache.h"     
2.基本方法：	

	1）- (void)sd_setImageWithURL:(nullable NSURL *)url；
	2）- (void)sd_setImageWithURL:(nullable NSURL *)url  placeholderImage:(nullable UIImage *)placeholder；
	3）- (void)sd_setImageWithURL:(nullable NSURL *)url  placeholderImage:(nullable UIImage *)placeholder	options:(SDWebImageOptions)options；
	...
	...	
3.下载流程:		
![sdw4](sdw4.png)		

	1.入口：setImageWithURL:placeholderImage:options:会先把 placeholderImage 显示，然后 SDWebImageManager 根据 URL 开始处理图片;
	2.进入：SDWebImageManagerdownloadWithURL:delegate:options:userInfo:，交给 SDImageCache 从缓存查找图片是否已经下载queryDiskCacheForKey:delegate:userInfo:  ；
	3.先从内存图片缓存查找是否有图片，如果内存中已经有图片缓存，SDImageCacheDelegate 回调imageCache:didFindImage:forKey:userInfo: 到 SDWebImageManager；
	4.SDWebImageManagerDelegate 回调webImageManager:didFinishWithImage:到 UIImageView+WebCache 等前端展示图片；
	5.如果内存缓存中没有，生成 NSInvocationOperation添加到队列开始从硬盘查找图片是否已经缓存；
	6.根据 URLKey 在硬盘缓存目录下尝试读取图片文件。这一步是在 NSOperation 进行的操作，所以回主线程进行结果回调 notifyDelegate: ；
	7.如果上一操作从硬盘读取到了图片，将图片添加到内存缓存中（如果空闲内存过小，会先清空内存缓存）。 SDImageCacheDelegate 回调 imageCache:didFindImage:forKey:userInfo:。进而回调展示图片；
	8.如果从硬盘缓存目录读取不到图片，说明所有缓存都不存在该图片，需要下载图片，回调imageCache:didNotFindImageForKey:userInfo: ；
	9.共享或重新生成一个下载器 SDWebImageDownloader 开始下载图片；
    10.图片下载由NSURLSession(NSURLConnection已经被剔除了) 来做，实现相关 delegate 来判断图片下载中、下载完成和下载失败；
		session:didReceiveData: 中利用 ImageIO做了按图片下载进度加载效果；
		sessionDidFinishLoading:数据下载完成后交给SDWebImageDecoder 做图片解码处理；
    11图片解码处理在一个NSOperationQueue完成，不会拖慢主线程 UI。如果有需要对下载的图片进行二次处理，最好也在这里完成，效率会好很多。
	12在主线程 notifyDelegateOnMainThreadWithInfo: 宣告解码完成，		imageDecoder:didFinishDecodingImage:userInfo:回调给 SDWebImageDownloader;
		imageDownloader:didFinishWithImage:回调给 SDWebImageManager 告知图片下载完成;
	 13通知所有的 downloadDelegates下载完成，回调给需要的地方展示图片;
	14将图片保存到 SDImageCache 中，内存缓存和硬盘缓存同时保存;
	15写文件到硬盘也在以单独 NSInvocationOperation 完成，避免拖慢主线程;
	16SDImageCache 在初始化的时候会注册一些消息通知，在内存警告或退到后台的时候清理内存图片缓存，应用结束的时候清理过期图片。
	17SDWI 也提供了 UIButton+WebCache 和 MKAnnotationView+WebCache，方便使用；
	18SDWebImagePrefetcher 可以预先下载图片，方便后续使用；

四、SDWebImage图片格式		
1.格式及代码		

	JPEG 0xFF
	PNG 0x89
	GIF   0x47
	TIFF 0x49或0x4D
	R as RIFF for WEBP 0x52
2.格式枚举SDImageFormat
	 	
    SDImageFormatUndefined = -1,
    SDImageFormatJPEG = 0,
    SDImageFormatPNG,
    SDImageFormatGIF,
    SDImageFormatTIFF,
    SDImageFormatWebP,
    SDImageFormatHEIC
3.格式方法:
	
```objectivec
+ (SDImageFormat)sd_imageFormatForImageData:(nullable NSData *)data {
    if (!data) {
        return SDImageFormatUndefined;
    }
    
    // File signatures table: http://www.garykessler.net/library/file_sigs.html
    uint8_t c;
    [data getBytes:&c length:1];
    switch (c) {
        case 0xFF:
            return SDImageFormatJPEG;
        case 0x89:
            return SDImageFormatPNG;
        case 0x47:
            return SDImageFormatGIF;
        case 0x49:
        case 0x4D:
            return SDImageFormatTIFF;
        case 0x52: {
            if (data.length >= 12) {
                //RIFF....WEBP
                NSString *testString = [[NSString alloc] initWithData:[data subdataWithRange:NSMakeRange(0, 12)] encoding:NSASCIIStringEncoding];
                if ([testString hasPrefix:@"RIFF"] && [testString hasSuffix:@"WEBP"]) {
                    return SDImageFormatWebP;
                }
            }
            break;
        }
        case 0x00: {
            if (data.length >= 12) {
                //....ftypheic ....ftypheix ....ftyphevc ....ftyphevx
                NSString *testString = [[NSString alloc] initWithData:[data subdataWithRange:NSMakeRange(4, 8)] encoding:NSASCIIStringEncoding];
                if ([testString isEqualToString:@"ftypheic"]
                    || [testString isEqualToString:@"ftypheix"]
                    || [testString isEqualToString:@"ftyphevc"]
                    || [testString isEqualToString:@"ftyphevx"]) {
                    return SDImageFormatHEIC;
                }
            }
            break;
        }
    }
    return SDImageFormatUndefined;
}
```	


五、SDWebImage枚举	
1.SDWebImageOptions    

     //默认的情况下，当一个URL下载失败了，那么这个URL会被放在黑名单里面，不会继续尝试，这个标志位就是使黑名单效果失效，保证失败后可以重新尝试。
    SDWebImageRetryFailed = 1 << 0,
	
	//默认情况下，在UI交互的情况下就开始图像下载，该标志位使该特性失效，例如在UIScrollView减速时延迟下载。
    SDWebImageLowPriority = 1 << 1,
	
	//该标志位使磁盘缓存失效，只是内存缓存。
    SDWebImageCacheMemoryOnly = 1 << 2,
	
	//该标志位可以确保下载的回调，图像可以边下载边显示
    SDWebImageProgressiveDownload = 1 << 3,
	
	//刷新缓存
    SDWebImageRefreshCached = 1 << 4,
	
	//后台下载
    SDWebImageContinueInBackground = 1 << 5,
	
	//处理NSHTTPCookieStore中存储的cookie
    SDWebImageHandleCookies = 1 << 6,
	
	//允许使用无效的SSL整数，但是仅用于测试，生产环境要小心
    SDWebImageAllowInvalidSSLCertificates = 1 << 7,
	
	//默认的情况，图像下载都是按照队列内部的顺序进行的，这个标志位就是提高优先级，让其移动到队列的首位。
    SDWebImageHighPriority = 1 << 8,
   
    //默认的情况下是占位图在下载图像的时候加载，这个标志位延迟占位图的加载，直到图像下载完成。
    SDWebImageDelayPlaceholder = 1 << 9,
	
	//我们一般不调用transformDownloadedImage代理方法处理动画图片，因为大多数的转换代码会损坏它，使用这个标志位可以转换它们。
    SDWebImageTransformAnimatedImage = 1 << 10,
    
    //默认情况下，图像在下载完成后都是加载到UIImageview上，但是在一些情况下，我们想要在设置图像之前处理它，比如说加一个滤镜或者褪色的动画效果等等，使用这个标志位，如果你想要手动的在成功完成回调后设置这个图片。
    SDWebImageAvoidAutoSetImage = 1 << 11,
  
    //默认情况下，图像都是根据他们原始尺寸进行解码，在ios中，这个标志位会缩小图片，以便能够兼容器件内存的限制，如果SDWebImageProgressiveDownload被设置了，那么这个位就失效了。
    SDWebImageScaleDownLargeImages = 1 << 12，

    SDWebImageQueryDataWhenInMemory = 1 << 13,
    
    /**
     * By default, we query the memory cache synchronously, disk cache asynchronously. This mask can force to query disk cache synchronously to ensure that image is loaded in the same runloop.
     * This flag can avoid flashing during cell reuse if you disable memory cache or in some other cases.
     */
    SDWebImageQueryDiskSync = 1 << 14,
    
    /**
     * By default, when the cache missed, the image is download from the network. This flag can prevent network to load from cache only.
     */
    SDWebImageFromCacheOnly = 1 << 15,
    /**
     * By default, when you use `SDWebImageTransition` to do some view transition after the image load finished, this transition is only applied for image download from the network. This mask can force to apply view transition for memory and disk cache as well.
     */
    SDWebImageForceTransition = 1 << 16
2.SDWebImageDownloaderExecutionOrder - 设置操作的执行顺序    

	//队列模式 先进先出，这个是默认值 
	SDWebImageDownloaderFIFOExecutionOrder, 
	 
	//堆栈模式，后进先出 
	SDWebImageDownloaderLIFOExecutionOrder 
3.填充模式UIViewContentMode		

	 UIViewContentModeScaleToFill,
    UIViewContentModeScaleAspectFit,      // contents scaled to fit with fixed aspect. remainder is transparent
    UIViewContentModeScaleAspectFill,     // contents scaled to fill with fixed aspect. some portion of content may be clipped.
    UIViewContentModeRedraw,              // redraw on bounds change (calls -setNeedsDisplay)
    UIViewContentModeCenter,              // contents remain same size. positioned adjusted.
    UIViewContentModeTop,
    UIViewContentModeBottom,
    UIViewContentModeLeft,
    UIViewContentModeRight,
    UIViewContentModeTopLeft,
    UIViewContentModeTopRight,
    UIViewContentModeBottomLeft,
    UIViewContentModeBottomRight,
    解析:
    	填充模式的枚举，每次使用的时候我们都只能使用其中的一个元素，比如使用UIViewContentModeScaleToFill，而且在定义枚举值的时候我们可以指定从一个整数值开始，后面的值依次比前一个值加1；如果不指定值，那么第一个成员的值默认为0，后面的值还是依次递增为1， 2......，这个使用起来很简单了，也不用多说;
4.位移枚举		
eg:UIViewAnimationOptions      

	UIViewAnimationOptionLayoutSubviews            = 1 <<  0,
    UIViewAnimationOptionAllowUserInteraction      = 1 <<  1, // turn on user interaction while animating
    UIViewAnimationOptionBeginFromCurrentState     = 1 <<  2, // start all views from current value, not initial value
    UIViewAnimationOptionRepeat                    = 1 <<  3, // repeat animation indefinitely
    UIViewAnimationOptionAutoreverse               = 1 <<  4, // if repeat, run animation back and forth
    UIViewAnimationOptionOverrideInheritedDuration = 1 <<  5, // ignore nested duration
    UIViewAnimationOptionOverrideInheritedCurve    = 1 <<  6, // ignore nested curve
    UIViewAnimationOptionAllowAnimatedContent      = 1 <<  7, // animate contents (applies to transitions only)
    UIViewAnimationOptionShowHideTransitionViews   = 1 <<  8, // flip to/from hidden state instead of adding/removing
    UIViewAnimationOptionOverrideInheritedOptions  = 1 <<  9, // do not inherit any options or animation type
    
    UIViewAnimationOptionCurveEaseInOut            = 0 << 16, // default
    UIViewAnimationOptionCurveEaseIn               = 1 << 16,
    UIViewAnimationOptionCurveEaseOut              = 2 << 16,
    UIViewAnimationOptionCurveLinear               = 3 << 16,
    
    UIViewAnimationOptionTransitionNone            = 0 << 20, // default
    UIViewAnimationOptionTransitionFlipFromLeft    = 1 << 20,
    UIViewAnimationOptionTransitionFlipFromRight   = 2 << 20,
    UIViewAnimationOptionTransitionCurlUp          = 3 << 20,
    UIViewAnimationOptionTransitionCurlDown        = 4 << 20,
    UIViewAnimationOptionTransitionCrossDissolve   = 5 << 20,
    UIViewAnimationOptionTransitionFlipFromTop     = 6 << 20,
    UIViewAnimationOptionTransitionFlipFromBottom  = 7 << 20,

    UIViewAnimationOptionPreferredFramesPerSecondDefault     = 0 << 24,
    UIViewAnimationOptionPreferredFramesPerSecond60          = 3 << 24,
    UIViewAnimationOptionPreferredFramesPerSecond30          = 7 << 24,
    解析:
    	这种枚举我们就可以一次选择多个值，比如说我们可以使用UIViewAnimationOptionLayoutSubviews | UIViewAnimationOptionAllowUserInteraction，这么写我们就是使用该枚举的前两个成员。这样写的作用就是既可以布局子视图，还可以允许用户交互。
	位移枚举的原理:
		其实给一个枚举变量赋予多个枚举值的时候，原理只是把各个枚举值加起来罢了。当加起来以后，就获取了一个新的值，那么为了保证这个值的唯一性，这个时候就体现了位运算的重要作用。位运算可以确保枚举值组合的唯一性。其实|这个按位或的符号就是将多个枚举值加起来的意思。
	  ● 两个不同的枚举值按位与&其实就是0，相同的按位&就是其本身;
	  ● 两个不同的枚举值按位或|其实就是两个枚举值的相加，相同的按位或|就是其本身;
	
	
	
参考资料：		
1.[内存缓存和磁盘缓存区别](https://www.jianshu.com/p/3b0e290cc049)       
2.[内存缓存和磁盘缓存](https://blog.csdn.net/u010958446/article/details/57409138)       
3.[SDWebImage](https://cocoapods.org/?q=sdwebimage)      
4.[SDWebImage — 图片类型判断深入研究](https://www.jianshu.com/p/189f186f767f)      
5.[位移枚举](https://www.jianshu.com/p/3a046ae71b59)       
		