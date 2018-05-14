---
title: 微信小程序-需求-TabBar
date: 2018-05-14 13:43:21
tags: 微信小程序
categories: 小程序
---

一、需求描述		
![选择角色入口](tabbar1.png)			
![tabbar渲染](tabbar2.png)			
在入口处有三个角色，点击不同的角色在Tabbar上显示不同的内容(pagPpath、text、iconPath、selectedIconPath):

二、解决方案：		
1.自定义Tabbar模版

	1.自定义TabBar模版；
	2.app.json中不做“tabBar”的配置；
	3.app.js中:
		globalData:{
			userInfo: null,
			...
			tabBarParent:{
				"color": "#9e9e9e";
				...
				“list”: 
				[{
					"pagePath": "/pages/home/home",
          		"text": "学校",
          		"iconPath": "/pages/images/home.png",
          		"selectedIconPath": "/pages/images/home-sel.png",
          		"selectedColor": "#4EDF80",
					"clas": "menu-item",
       	        active: false
               },
               {
               	通讯录
               	...
					"clas": "menu-item",
       	        active: false
					},
					{班级圈...},
					{我的...}]
			},
			tabBarTeacher:{//图标和家长端不一样，pagePath不一样
				"color": "#9e9e9e";
				...
				“list”: 
				[{	学校
					"clas": "menu-item1",
       	        active: false
				},
				{通讯录},
				{班级圈},
				{我}]
			},
			tabBarAdmin:{//图标和家长端不一样，pagePath不一样
				"color": "#9e9e9e";
				...
				“list”: [
				{学校},{信箱},{学校公告},{中心}
				]
			},
		}
	4.app.js中重写系统方法：
	//重写家长中心
	updateTabBarParent:funcation() {
		var _curPageArr = getCurrentPages();
	    var _curPage = _curPageArr[_curPageArr.length - 1];
	    var _pagePath = _curPage.__route__;
	    if (_pagePath.indexOf('/') != 0) {
	      _pagePath = '/' + _pagePath;
	    }
	    var tabBar = this.globalData.tabBar;
	    for (var i = 0; i < tabBar.list.length; i++) {
	      tabBar.list[i].active = false;
	      if (tabBar.list[i].pagePath == _pagePath) {
	        tabBar.list[i].active = true;//根据页面地址设置当前页面状态    
	      }
	    }
	    _curPage.setData({
	      tabBar: tabBar
	    });
	}
	//重写教师中心
	updateTabBarTeacher:funcation() {
		...
	}
	//重写管理中心
	updateTabBarAdmin:funcation() {
		...
	}
	5.在相关模块文件xx..wxml中导入模版：
	eg:home.wxml
	<import src="../template/template.wxml"/> <template is="tabBar" data="{{tabBar}}"/>
	6.在相关模块文件xx.js中刷新tabBar：
	eg:home.js
	Page({
		
		...
		onLoad:function (option){
			app.updateTabBarParent:funcation();
		}
		...
	})
	
2.定义变量，配置变量(系统方案，验证中)       
app.json配置

	{
	  "pages": [
	    ...
	  ],
	  "window": {
	    ...
	  },
	  "tabBar": {
	    "color": "#6e6d6b",
	    "selectedColor": "#33cbd5",
	    "list": [{
	      "pagePath": "pages/common/home/home",
	      "text": "学校",
	      "iconPath": "pages/images/nav/home.png",//默认图标
	      "selectedIconPath": "pages/images/nav/home-sel.png"//选中图标
	    },
	  ...
	  }
	}
变量验证

待完善



参考资料：		
1.[自定义Tabbar模版](https://blog.csdn.net/qq_29729735/article/details/78933721)     
2.[官网Tabbar组件](https://developers.weixin.qq.com/miniprogram/dev/framework/config.html)     
3.[自定义Tabbar](https://blog.csdn.net/alertqian/article/details/71711453)