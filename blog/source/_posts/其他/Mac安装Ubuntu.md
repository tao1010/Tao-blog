---
title: Mac安装Ubuntu
toc: true
date: 2018-06-22 14:02:44
tags: 系统
categories: 其他
---

1.[下载Ubuntu](https://www.ubuntu.com/download/desktop)<br>

<font color='red'>未成功，待进进一步尝试...</font>

<!-- more -->

2.磁盘分区

	1.打开 Mac 中的磁盘工具；
	2.选择最外层磁盘，点击工具栏上的 分区；
	3.编辑分区的名称 "Linux" 和 设置分区大小 60G；
	4.点击应用，等待分区完成。
![ubuntu1](ubuntu1.png)<br>
![ubuntu2](ubuntu2.png)<br>
3.ubuntu.iso转换成dmg格式
	
	cd xxx/test/ #test文件夹下放置之前下载的ubuntu-18.04-desktop-amd64.iso 文件 
	hdiutil convert ubuntu-18.04-desktop-amd64.iso -format UDRW -o ubuntu.dmg 
4.制作启动盘

	1.插入U盘
	2.终端：
		diskutil list #查看mac下磁盘序号
![disk](disk.png)<br>		

	3.终端：将 N 改成U盘序号(通常是1或2，上述2结果中的序号)
		diskutil unmountDisk /dev/diskN
		显示：
		Unmount of all volumes on disk1 was successful
	4.终端：将 N 改成U盘序号(通常是1或2，上述2结果中的序号)
		sudo dd if=ubuntu.dmg of=/dev/rdiskN bs=2m
		显示：
		Password:
		916+1 records in
		916+1 records out
		1921843200 bytes transferred in 304.902708 secs (6303136 bytes/sec)
	5.终端：退出U盘，将 N 改成U盘序号(通常是1或2，上述2结果中的序号)
	diskutil eject /dev/diskN
	显示：
	Disk /dev/disk1 ejected
5.安装Mac的引导工具 Refind


	
参考资料：<br>
1.[mac上安装ubuntu双系统](http://www.cnblogs.com/diligenceday/p/6103530.html)<br>
2.[在 MacBook Pro 上安装 Ubuntu 16.04.1 LTS](https://www.linuxidc.com/Linux/2017-04/143112.htm)<br>
3.[Mac上安装ubuntu](https://www.cnblogs.com/diligenceday/p/6103530.html)<br>
4.[Mac OS + Ubuntu + Windows](https://blog.csdn.net/babytang008/article/details/70879634)<br>
5.[Mac引导工具 Refind](https://blog.csdn.net/xiaoshaxs?t=1)<br>
6.[Refind](http://www.pc6.com/mac/113059.html)<br>