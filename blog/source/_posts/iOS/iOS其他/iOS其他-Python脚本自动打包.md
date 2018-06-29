---
title: iOS其他-脚本自动打包
toc: true
date: 2018-06-29 09:17:57
tags: iOS其他
categories: iOS
---

脚本打包的使用场景：
	
	主要用于测试分发，正式发布建议使用手动打包(对脚本没有掌握情况下)；
	企业账号和其他账号使用的不同的脚本文件；
	自动打包的大小和手动打包的大小有区别(打包效率和包的优化上有所取舍)；
	


一、Python脚本打包<br>

<!-- more -->

1.缺点

	配置内容较多
	目前只支持 .xcodeproj 的工程,	不支持 .xcworkspace 的工程
	
	

二、jenkins 脚本打包




三、shell脚本打包



参考资料:<br>
一、Python脚本打包<br>
1.[Python脚本-iOS自动打包](https://mp.weixin.qq.com/s/5f2Pd8RHBN7TClZDZlx_Gw)<br>
2.[Python脚本-iOS自动打包2](https://www.jianshu.com/p/1f47066da6f7)<br>
3.[Python 打包脚本](https://github.com/hades0918/ipapy)<br>
4.[蒲公英Fir分发平台](https://github.com/FIRHQ/fir-cli/blob/master/README.md)<br>
5.[Python自动打包](https://www.jianshu.com/p/1f47066da6f7)
二、jenkins 脚本打包<br>
1.[Jenkins集成iOS全自动打包](https://www.jianshu.com/p/6a3a009da35b)<br>

三、shell脚本打包<br>
1.[详解Shell脚本实现iOS自动化编译打包提交](http://zackzheng.info/2015/12/27/2015-12-27-an-automated-script-for-building-archiving-submission-sending-emails/)