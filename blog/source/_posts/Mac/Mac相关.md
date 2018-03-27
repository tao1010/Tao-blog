---
title: Mac相关
date: 2018-03-27 14:49:46
tags: Mac
categories: Mac
---

一、软件安装

1.文件损坏   
![file_bad](file_bad.jpg)

	原因:系统安全机制，默认不允许用户安装自行下载的程序;
	解决:1.终端输入
```bash
sudo spctl --master-disable
```
	2.终端输入开机密码
	3.再次打开软件