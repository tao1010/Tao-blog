---
title: windows系统忘记登录密码
date: 2018-05-14 13:07:21
tags: 破解
categories: 其他
---

方法一：WinPE清除密码

	1.PE盘启动电脑；
	2.C盘：windows/system32下：
		更改Magnify.exe 和cmd.exe的所有者: administrators;
		更改Magnify.exe 和cmd.exe的权限 administrators 为完全控制；
		改名Magnify.exe-->Magnify.exe1
			cmd.exe-->Magnify.exe
	3.更改密码
		重启Win7；
		启用放大镜；
		命令：
			net user（查看用户名）
			net user 用户名新密码
			激活管理员账号使用： net user administrator /active:yes
		改回之前改动的操作
	

方法二：重装系统	

[老毛桃](http://www.laomaotao.org/)		
[雨林木风](http://win.sysdaa.com/)

tips:		
1.U盘启动工具制作；		
	
	老毛桃
	雨林木风
2.U盘重装系统；



参考值资料：		
1.[WinPE清除密码](https://www.jianshu.com/p/9ad268ed695f)		
2.[Win7找回密码](https://www.jianshu.com/p/53a101a0f24b)	
3.[破解系统密码](http://www.360doc.com/content/17/0930/07/32695421_691262633.shtml)		
4.[管理员密码破解](http://www.xitongcheng.cc/xtjc/7866.html)		
5.[重置密码-错误警告](https://zhidao.baidu.com/question/921626187192022619.html)		
6.[windows管理员密码破解](https://zhidao.baidu.com/question/1754588586738700428.html)