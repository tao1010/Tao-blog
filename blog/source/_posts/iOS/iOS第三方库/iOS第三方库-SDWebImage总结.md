---
title: iOS第三方库-SDWebImage总结
toc: true
date: 2018-07-06 09:10:21
tags: iOS第三方库
categories: iOS
---

一、SDWebImage怎么实现缓存的

<!-- more -->

1.过程
	
	分为：
		内存缓存 - 利用SDWebImageCache类的NSCache属性
		磁盘缓存 - 利用NSFileManager
		操场缓存 - 利用runtime关联的字典属性
	下载前先查询缓存，没有就下载，下载后保存图片到缓存


二、SDWebImage下载后的图片在什么时候用到解码<br>


三、怎样安全地在主线程执行一个Block？


四、怎样区分SDWebImageDownloader和SDWebImageManager的工作？


参考资料:<br>
1.[SDWebImage总结](https://www.jianshu.com/p/b0f071672ef8)