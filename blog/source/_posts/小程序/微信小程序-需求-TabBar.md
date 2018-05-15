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
	3.app.wxss中修改自定义模版样式：
		.menu-item{  
		  width: 24%;  
		  float: left;  
		  text-align: center;  
		  padding-top: 8px;  
		}  
		.img{  
		  width: 23px;  
		  height: 23px;  
		  display: block;  
		  margin:auto;  
		}  
		.clear{  
		  clear: both;  
		}  
		.tab-bar{  
		
		  width:100%;
		  position: fixed;
		  bottom:0;
		  padding:10rpx;
		  /* margin-left:-4rpx; */
		  background:#F7F7FA;
		  font-size:20rpx;
		  color:#8A8A8A;
		  /* box-shadow: 6rpx 6rpx 6rpx 6rpx #aaa; */
		  height: 40px;
		
		
		  /* position: fixed;  
		  width: 100%;  
		  padding: 0px 2%;  
		  height: 50px;
		  background: rgb(247, 247, 250);
		  font-size: 20rpx; */
		}
	4.app.js中:
		globalData:{
			userInfo: null,
			userRole: "-1",//1-家长 2-教师 3-管理员|学校
			...
			tabBarParent:{
				"color": "#9e9e9e",
				"selectedColor": "#33cbd5",
		      "postion": "bottom",
      			"backgroundColor": "#fff",
		      "borderStyle": "#ccc",
				“list”: 
				[{
					"pagePath": "/pages/home/home",
          		"text": "学校",
          		"iconPath": "/pages/images/home.png",
          		"selectedIconPath": "/pages/images/home-sel.png",
          		"selectedColor": "#4EDF80",//配置选中字体颜色
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
	    var tabBar = this.globalData.tabBarParent;//获取家长Tabbar数组数据
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
	5.设置入口角色
	index.html
	<view class="container">
	    <view class='button' bindtap='clickParent'>家长中心</view>
	    <view class='button' bindtap='clickTeacher'>教师中心</view>
	    <view class='button' bindtap='clickAdmin'>管理中心</view>
	</view>
	
	index.js
	clickParent: function(){

	    console.log("家长");
	    getApp().globalData.userRole = "1"
	    wx.redirectTo({
	      url: '../parent/parent',
	    })
	  },
	  clickTeacher: function(){
	
	    console.log("教师");
	    getApp().globalData.userRole = "2"
	    wx.redirectTo({
	      url: '../teach/teach',
	    })
	  },
	  clickAdmin: function(){
	
	    console.log("管理");
	    getApp().globalData.userRole = "3"
	    wx.redirectTo({
	      url: '../admin/admin',
	    })
	  },
		
	6.在相关模块文件xx..wxml中导入模版：
	eg:home.wxml
	<import src="../template/template.wxml"/> <template is="tabBar" data="{{tabBar}}"/>
	7.在相关模块文件xx.js中刷新tabBar：
	eg:home.js
	const app = getApp()
	Page({
		
		...
		onLoad:function (option){
			if (getApp().globalData.userRole === "1") {
		      app.updateTabBarWithParent();
		    }
		    if (getApp().globalData.userRole === "2") {
		      app.updateTabBarWithTeacher();
		    }
		    if (getApp().globalData.userRole === "3") {
		      app.updateTabBarWithAdmin();
		    }
		}
		...
	})
	
2.定义变量，配置变量(      
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




参考资料：		
1.[自定义Tabbar模版](https://blog.csdn.net/qq_29729735/article/details/78933721)     
2.[官网Tabbar组件](https://developers.weixin.qq.com/miniprogram/dev/framework/config.html)     
3.[自定义Tabbar](https://blog.csdn.net/alertqian/article/details/71711453)