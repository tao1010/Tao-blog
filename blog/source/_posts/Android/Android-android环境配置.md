---
title: Android-android环境配置
toc: true
date: 2018-07-15 10:37:42
tags: android环境
categories: Android
---
一、卸载Mac上的Java

<!-- more -->

1.终端卸载
	
	必须以 root 用户身份或者使用 sudo 工具来执行删除命令.
	
	查看路径下是否存在下列文件
		/Library/Internet Plug-Ins/JavaAppletPlugin.plugin
		/Library/PreferencesPanes/JavaControlPanel.prefPane
		~/Library/Application Support/Java
	存在，执行:
		sudo rm -fr /Library/Internet\ Plug-Ins/JavaAppletPlugin.plugin
		sudo rm -fr /Library/PreferencesPanes/JavaControlPanel.prefPane
		sudo rm -fr ~/Library/Application\ Support/Java
		
	完成上述步骤后，搜索：
		 /Library/Java/JavaVirtualMachines
	删除对应Java文件即可


参考资料:	  
1.[卸载Mac上的Java](https://blog.csdn.net/haozhugogo/article/details/54809545)