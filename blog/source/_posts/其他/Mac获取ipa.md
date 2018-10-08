---
title: Mac获取ipa
toc: true
date: 2018-07-21 00:49:19
tags: ipa
categories: 其他
---

1.工具准备

<!-- more -->

	Apple Configurator 2   Mac appstore下载即可
	登录appleId
2.步骤
	
	1.在Appstore下载相关App（以xx宝为例）到iPhone或iPad上;
	2.打开 Apple Configurator 2
	3.选中连接Mac的移动设备后，点击 Apple Configurator 2 工具菜单 ”添加“ --> 应用；
	4.搜索app-xx宝，选中，点击右下角添加，等待添加完成(如下图)；
	5.不要操作  Apple Configurator 2,切换到桌面;
	6.快捷键：command + shift + G 或者在Finder 中前往文件夹...
	7.输入路径：~/Library/Group Containers/K36BKF7T3D.group.com.apple.configurator/Library/Caches/Assets/TemporaryItems/MobileApps/
	8.拷贝出 上述路径中的ipa文件，之后在 Apple Configurator 2 工具的弹框中点击停止，即可；
![zhifubao](zhifubao.png)          
![done](done.png)       
3.获取资源

	1.修改上述获取的ipa文件，修改后缀名 ipa -->zip;
	2.解压zip文件，在完成的文件夹中打开Payload 文件夹，即可看到应用xx宝应用程序；
	3.选中该应用程序，右键显示包内容即可查看图片资源等；
4.获取图片资源
	
	1.找到包内容中的 Assets.car 文件；
	2.打开 “Assets提取工具”，导入 Assets.car 文件，添加到处文件夹，点击提取即可获取；
	
参考资料:     
1.[Mac获取ipa和相关资源](https://www.jianshu.com/p/fdb50d303ad6)       
2.[Assets提取工具](https://github.com/pcjbird/AssetsExtractor)