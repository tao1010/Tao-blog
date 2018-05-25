---
title: 微信小程序-组件-地图组件和位置API
date: 2018-05-24 16:26:04
tags: 微信小程序
categories: 小程序
---

一、Map组件			
1.属性		

|属性名|类型|默认值|描述|
|---|---|---|---|
| longitude |Number|地图组件的经纬度必填, 如果不填经纬度则默认值是北京的经纬度|中心经度|
| latitude | Number |地图组件的经纬度必填, 如果不填经纬度则默认值是北京的经纬度|中心纬度|
|scale| Number |16|缩放级别 取值范围5 - 18|
|markers|Array||标记点|
|covers| Array | |即将移除，使用markers|
|polyline| Array ||路线|
|circles| Array ||圆|
|controls| Array ||控件|
|include-points| Array ||缩放视野以包含所有给定的坐标点|
|show-location|Boolean||显示带有方向的当前定位点|
|bindmarkertap|EventHandle||点击标记点时触发|
|bindcallouttap| EventHandle ||点击标记对应的气泡时触发|
|bindcontroltap| EventHandle ||点击控件时触发|
|bindregionchange| EventHandle ||视野发生变化时触发|
|bindtap| EventHandle ||点击地图时触发|
|bindupdated| EventHandle ||在地图渲染更新完成时触发|

2.makers 标记点用于在地图上显示标记的位置

|属性|类型|必填|描述|备注|
|---|---|---|---|---|
|id|Number|否|标记点id|marker点击事件回调会返回此id，建议为每个marker设置上Number类型id，保证更新marker时有更好的性能|
|latitude|Number|是|纬度|浮点数 范围-90 ～ 90|
|longitude|Number|是|经度|浮点数 范围-180 ～ 180|
|title|String|否|标注点名||
|iconPath|String|是|显示的图标|项目目录下的图片路径，支持相对路径写法，以'/'开头则表示小程序根目录，也支持临时路径|
|rotate|Number|否|旋转角度|顺时针旋转的角度 范围 0～360 默认0|
|alpha|Number|否|标注透明度|默认1，无透明，范围0～1|
|width|Number|否|标注图标宽度|默认为图片实际宽度|
|height|Number|否|标注图标高度|默认为图片实际高度|
|callout|object|否|自定义标记点上方的气泡窗口|可识别换行符|
|label|object|否|为标记点旁边增加标签|可识别换行符|
|anchor|object|否|经纬度在标注图标的锚点，默认在底边中点|{x, y}，x表示横向(0-1)，y表示竖向(0-1)。{x: .5, y: 1} 表示底边中点|

3.makers 上的气泡 callout

|属性|类型|默认值|描述|
|---|---|---|---|
|content|String|文本||
|color|String|文本颜色||
|fontSize|Number|文字大小||
|borderRadius|NUmber|callout边框圆角||
|bgColor|String|背景颜色||
|padding|Number|文本边缘留白||
|display|String|'BYCLICK':点击显示; 'ALWAYS':常显|
|textAlign|String|文本对齐方式。有效值: left, right, center||

4.makers 上气泡 label

|属性名|类型|默认值|描述|
|---|---|---|---|
|content|String|文本||
|color|String|文本颜色||
|fontSize|Number|文字大小||
|x|Number|label的坐标，原点是marker对应的经纬度||
|y|Number|label的坐标，原点是marker对应的经纬度||
|borderWidth|Number|边框宽度||
|borderColor|String|边框颜色||
|borderRadius|NUmber|边框圆角||
|bgColor|String|背景颜色||
|padding|Number|文本边缘留白||
|textAlign|String|文本对齐方式。有效值: left, right, center||

5.polyline 指定一系列坐标系(从数组的第一项连线到最后一项)

|属性|类型|必填|描述|备注|
|---|---|---|---|---|
|points |Array|	是|	经纬度数组|	[{latitude: 0, longitude: 0}]	|
|color | String	|否	| 线的颜色|8位十六进制表示，后两位表示alpha值，如：#000000AA|
|width | Number|	否		|线的宽度|
|dottedLine | 	Boolean|	否| 是否虚线|	默认false|
|arrowLine | Boolean	|否	|	带箭头的线| 默认false，开发者工具暂不支持该属性|	
|arrowIconPath | String	|否|更换箭头图标	|在arrowLine为true时生效	|
|borderColor |	String|	否		|更换箭头图标|
|borderWidth |	Number|	否|线的厚度|
6.circles 在地铁上画圆

|属性|类型|必填|描述|备注|
|---|---|---|---|---|
|latitude|Number	|是	|维度|浮点数，范围 -90 ~ 90|
|longitude| Number	|是|经度|浮点数，范围 -180 ~ 180|
|color| String	|否	|描边的颜色 |8位十六进制表示，后两位表示alpha值，如：#000000AA|
|fillColor	|	String|	否| 填充颜色|	8位十六进制表示，后两位表示alpha值，如：#000000AA|
|radius|Number|	是|	半径|
|strokeWidth|	Number	|否|描边的宽度|

7.controls 在地图上显示控件，控件不随着地图移动

|属性|类型|必填|描述|备注|
|---|---|---|---|---|
|id|Number| 否 |控件id|在控件点击事件回调会返回此id|
|position| Object| 是|控件在地图的位置|	控件相对地图位置|
|iconPath| String| 是 | 显示的图标 | 项目目录下的图片路径，支持相对路径写法，以'/'开头则表示相对小程序根目录；也支持临时路径|
|clickable| Boolean|否|是否可点击|	默认不可点击|
8.position

|属性|类型|必填|描述|备注|
|---|---|---|---|---|
|left|Number|否|距离地图的左边界距离|默认0|
|top|Number|否|距离地图的上边界距离|默认0|
|width|Number|否|控件宽度|默认为图片宽度|
|height|Number|否|控件高度|默认为图片高度|

9.tips

	map 组件是由客户端创建的原生组件，它的层级是最高的，不能通过 z-index 控制层级；
	请勿在 scroll-view、swiper、picker-view、movable-view 中使用 map 组件；
	css 动画对 map 组件无效；
	map 组件使用的经纬度是火星坐标系，调用 wx.getLocation 接口需要指定 type 为 gcj02；
	地图组件的经纬度必填, 如果不填经纬度则默认值是北京的经纬度；
10.eg

	wxml
	  <view class='map'>
	    <map id='map' scale='16' 
	      longitude='{{longitude}}'
	      latitude='{{latitude}}'
	      controls='{{controls}}'
	      markers='{{markers}}'
	      polyline='{{polyline}}'
	      style='width: 100%; height: 100%'
	      bindcontroltap='bindcontroltap'
	      bindtap='bindtap'
	      bindupdated='bindupdated'
	      bindmarkertap='bindmarkertap'
	      show-location='true'
	      bindcallouttap='bindcallouttap'
	      bindregionchange='bindregionchange'>
	    </map>
	  </view>

	 js
	 	
	 	  bindregionchange: function(){

		    console.log('视野发生变化时触发...')
		  },
		  bindcallouttap: function(){
		
		    console.log('点击标记点对应的气泡时触发...')
		  },
		  bindtap: function(){
		
		    console.log('点击地图时触发...')
		  },
		  bindupdated: function(){
		
		    console.log('在地图渲染更新完成时触发...')
		  },
		  bindcontroltap: function(){
		
		    console.log('点击控件时触发...')
		  },
		  bindmarkertap: function(){
		
		    console.log('点击标记点时触发...')
	      },
  

二、位置API			
1.获取位置		
wx.getLocation(object) - 获取当前的地理位置、速度   
	
	当用户离开小程序后，此接口无法调用；当用户点击“显示在聊天顶部”时，此接口可继续调用； 
参数说明

|参数|类型|必填|描述|
|---|---|---|---|
|object参数||||
| type |String|否|默认为 wgs84 返回 gps 坐标，gcj02 返回可用于wx.openLocation的坐标|
| altitude |Boolean|否|传入 true 会返回高度信息，由于获取高度需要较高精确度，会减慢接口返回速度|
| success |Function|是|调用成功的回调函数|
| fail | Function |否|调用失败的回调函数|
| complete | Function |否|调用结束的回调函数（调用成功、失败都会执行）|
|success返回参数||||
| latitude |||纬度，浮点数，范围为-90~90，负数表示南纬|
| longitude |||经度，浮点数，范围为-180~180，负数表示西经|
| speed |||速度，浮点数，单位m/s|
| accuracy |||位置的精确度|
| altitude |||高度，单位 m|
| verticalAccuracy |||垂直精度，单位 m（Android 无法获取，返回 0）|
| horizontalAccuracy |||垂直精度，单位 m（Android 无法获取，返回 0）|
 
wx.chooseLocation(object) - 打开地图选择位置    
参数说明：		

|参数|类型|必填|描述|
|---|---|---|---|
|object参数||||
| success |Function|是|调用成功的回调函数|
| fail | Function |否|调用失败的回调函数|
| complete | Function |否|调用结束的回调函数（调用成功、失败都会执行）|
|success返回参数||||
| latitude |||纬度，浮点数，范围为-90~90，负数表示南纬|
| longitude |||经度，浮点数，范围为-180~180，负数表示西经|
| name |||位置名|
| address |||详细地址|

eg:

	wx.getLocation({
	  type: 'wgs84',
	  success: function(res) {
	    var latitude = res.latitude
	    var longitude = res.longitude
	    var speed = res.speed
	    var accuracy = res.accuracy
	  }
	})
2.查看位置		
wx.openLocation(object) - 使用微信内置地图查看位置     
参数说明

|参数|类型|必填|描述|
|---|---|---|---|
| scale |INT|否|缩放比例，范围5~18，默认为18|
| latitude |Float|是|纬度，浮点数，范围为-90~90，负数表示南纬|
| longitude |Float|是|经度，浮点数，范围为-180~180，负数表示西经|
| name |String|否|位置名|
| address |String|否|详细地址|
| success |Function|是|调用成功的回调函数|
| fail | Function |否|调用失败的回调函数|
| complete | Function |否|调用结束的回调函数（调用成功、失败都会执行）|
eg：
	
	wx.getLocation({
	  type: 'gcj02', //返回可以用于wx.openLocation的经纬度
	  success: function(res) {
	    var latitude = res.latitude
	    var longitude = res.longitude
	    wx.openLocation({
	      latitude: latitude,
	      longitude: longitude,
	      scale: 28
	    })
	  }
	})
	
3.组图组件控制		
wx.createMapContext(mapId,this) 

	创建并返回 map 上下文 mapContext 对象;
	在自定义组件下，第二个参数传入组件实例this，以操作组件内 <map/> 组件;
	mapContext对象方法列表：
		getCenterLocation object 获取当前地图中心的经纬度，返回的是gcj02坐标系可用于wx.openLocation;
		moveToLocation 无参数 将地图中心移动到当前定位点，需要配合map组件的show-location使用
		translateMarker object  平移marker，带动画
		includePoints object 缩放视野展示所有经纬度
		getRegion object 获取当前地图的视野范围
		getScale object 获取当前地图的缩放级别
eg:
	
	<!-- map.wxml -->
	<map id="myMap" show-location />
	
	<button type="primary" bindtap="getCenterLocation">获取位置</button>
	<button type="primary" bindtap="moveToLocation">移动位置</button>
	<button type="primary" bindtap="translateMarker">移动标注</button>
	<button type="primary" bindtap="includePoints">缩放视野展示所有经纬度</button>
	
	// map.js
	Page({
	  onReady: function (e) {
	    // 使用 wx.createMapContext 获取 map 上下文
	    this.mapCtx = wx.createMapContext('myMap')
	  },
	  getCenterLocation: function () {
	    this.mapCtx.getCenterLocation({
	      success: function(res){
	        console.log(res.longitude)
	        console.log(res.latitude)
	      }
	    })
	  },
	  moveToLocation: function () {
	    this.mapCtx.moveToLocation()
	  },
	  translateMarker: function() {
	    this.mapCtx.translateMarker({
	      markerId: 0,
	      autoRotate: true,
	      duration: 1000,
	      destination: {
	        latitude:23.10229,
	        longitude:113.3345211,
	      },
	      animationEnd() {
	        console.log('animation end')
	      }
	    })
	  },
	  includePoints: function() {
	    this.mapCtx.includePoints({
	      padding: [10],
	      points: [{
	        latitude:23.10229,
	        longitude:113.3345211,
	      }, {
	        latitude:23.00229,
	        longitude:113.3345211,
	      }]
	    })
	  }
	})
4.注意
	
	iOS 6.3.30 type 参数不生效，只会返回 wgs84 类型的坐标信息；

参考资料：	
1.[地图](https://developers.weixin.qq.com/miniprogram/dev/component/map.html)		
2.[位置](https://developers.weixin.qq.com/miniprogram/dev/api/location.html)		