---
title: 微信小程序-组件-地图组件和位置API
date: 2018-05-24 16:26:04
tags: 微信小程序
categories: 小程序
---

一、Map组件			
1.属性		


2.makers 标记点用于在地图上显示标记的位置


3.makers 上的气泡 callout


4.makers 上气泡 label


5.polyline 指定一系列坐标系(从数组的第一项连线到最后一项)

6.circles 在地铁上画圆

7.controls 在地图上显示控件，控件不随着地图移动


8.tips

	map 组件是由客户端创建的原生组件，它的层级是最高的，不能通过 z-index 控制层级；
	请勿在 scroll-view、swiper、picker-view、movable-view 中使用 map 组件；
	css 动画对 map 组件无效；
	map 组件使用的经纬度是火星坐标系，调用 wx.getLocation 接口需要指定 type 为 gcj02；
	
二、位置API			
1.获取位置		
wx.getLocation(object) - 获取当前的地理位置、速度     
wx.chooseLocation(object) - 打开地图选择位置   


2.查看位置		
wx.openLocation(object) - 使用微信内置地图查看位置	

3.组图组件控制		
wx.createMapContext(mapId,this) - 创建并返回 map 上下文 mapContext 对象


参考资料：	
1.[地图](https://developers.weixin.qq.com/miniprogram/dev/component/map.html)		
2.[位置](https://developers.weixin.qq.com/miniprogram/dev/api/location.html)		