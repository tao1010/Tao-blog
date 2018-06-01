---
title: 微信小程序-API-设备
date: 2018-05-31 13:18:30
tags: 微信小程序
categories: 小程序
---

一、系统信息 - 真机测试		
1.wx.getSystemInfo - 获取系统信息(异步)
	
	object参数说明：
		success/fail/complete 回调函数
	success参数说明：
		SDKVersion:"2.0.9"//客户端基础库版本
		batteryLevel:75//电池电量
		brand:"iPhone" //手机品牌
		errMsg:"getSystemInfo:ok"
		fontSizeSetting:16//用户字体大小设置。以“我-设置-通用-字体大小”中的设置为准，单位：px
		language:"zh_CN"//微信设置的语言
		model:"iPhone 7<iPhone9,1>"//手机型号
		pixelRatio:2//设备像素比
		platform:"ios"//客户端平台
		screenHeight:667
		screenWidth:375//屏幕宽度
		statusBarHeight:20//状态栏的高度
		system:"iOS 11.3.1"//操作系统版本
		version:"6.6.7"//微信版本号
		windowHeight:603
		windowWidth:375//可使用窗口宽度
2.wx.getSystemInfoSync() - 获取系统信息(同步)
	
	getSystemInfo: function(){
		//异步
	     wx.getSystemInfo({
	       success: function(res) {
	         console.log(res)
	       },
	     })
	     //同步
	    try{
	      var res = wx.getSystemInfoSync()
	      console.log(res)
	    }catch(e){
	      console.log(e)
	    }
  	},

3.wx.canIUse(String) - 判断小程序的API，回调，参数，组件等是否在当前版本可用

	string参数说明：
		 使用${API}.${method}.${param}.${options}或者
		 ${component}.${attribute}.${option}方式来调用，
	
		${API} 代表 API 名字
		${method} 代表调用方式，有效值为return, success, object, callback
		${param} 代表参数或者返回值
		${options} 代表参数的可选值
		${component} 代表组件名字
		${attribute} 代表组件属性
		${option} 代表组件属性的可选值
	eg：
		 caniUse: function(){

		    if (wx.canIUse('openBluetoothAdapter')){
		      console.log('I can')
		    }else{
		      console.log("You can't")
		    }
		  },
		  
		wx.canIUse('openBluetoothAdapter')
		wx.canIUse('getSystemInfoSync.return.screenWidth')
		wx.canIUse('getSystemInfo.success.screenWidth')
		wx.canIUse('showToast.object.image')
		wx.canIUse('onCompassChange.callback.direction')
		wx.canIUse('request.object.method.GET')
		wx.canIUse('live-player')
		wx.canIUse('text.selectable')
		wx.canIUse('button.open-type.contact')
二、网络状态		
1.wx.getNetworkType - 获取网络类型
	
	object参数说明：
		success/fail/complete 回调函数
	success参数说明：
		networkType 网络类型wifi/2g/3g/4g/unknown(Android下不常见的网络类型)/none(无网络)
	eg:
	 networkingStatus: function(){
	    wx.getNetworkType({
	      success: function(res) {
	        console.log(res)
	      },
	    })
 	 },
2.we.onNetworkStatusChange - 监听网络状态变化
	
	callback参数说明：
		isConnected 当前是否有网络连接 Boolean
		networkType 网络类型wifi/2g/3g/4g/unknown(Android下不常见的网络类型)/none(无网络)
	eg:
	networkingStatus: function(){
	    wx.onNetworkStatusChange(function(res){
	      console.log(res)
	    })
 	 },
三、屏幕亮度、截屏、振动		
1.wx.getScreenBrightness - 获取屏幕亮度
	
	getScreenBrightness 接口若安卓系统设置中开启了自动调节亮度功能，则屏幕亮度会根据光线自动调整，该接口仅能获取自动调节亮度之前的值，而非实时的亮度值
	object参数说明：
		success/fail/complete 回调函数
	success参数说明:
		value 屏幕亮度值，范围 0~1，0 最暗，1 最亮 Number
	eg：
	screenBrightness: function(){

	    wx.getScreenBrightness({
	
	      success: function(res){
	        console.log(res)
	      }    
	    })
	},
2.wx.setScreenBrightness - 设置屏幕亮度
	
	object参数说明：
		value 屏幕亮度值，范围 0~1，0 最暗，1 最亮 Number
		success/fail/complete 回调函数
	eg:
	wx.setScreenBrightness({
      value: 1,
      success: function(res){
        console.log(res)
      }
    })
3.wx.vibrateLong - 使手机较长振动（400ms）
	
	object参数说明：
		success/fail/complete 回调函数
	eg:
	vibrateLong: function(){
	    wx.vibrateLong({
	      success: function(res){
	          console.log('手机长振动了')
	      }
	    })
	},
4.wx.vibrateShort - 使手机发生较短振动（15ms）
	
	vibrateShort 接口仅在 iPhone7/iPhone7Plus 及 Android 机型生效；
	object参数说明：
		success/fail/complete 回调函数
	eg:
	vibrateShort: function(){
	    wx.vibrateShort({
	      success: function(res){
	          console.log('手机短振动了')
	      }
	    })
	},
5.wx.onUserCaptureScreen - 监听用户主动截屏事件

	用户使用系统截屏按键截屏时触发此事件;
	eg:
	wx.onUserCaptureScreen(function(res){
	    console.log('用户截屏了：' + res)
  	})
四、手机联系人、拨打电话、扫码、剪贴板		
1.wx.addPhoneContact - 更新手机通讯录联系人和联系方式的增加

	用户可以选择将该表单以“新增联系人”或“添加到已有联系人”的方式，写入手机系统通讯录，完成手机通讯录联系人和联系方式的增加
	object参数说明：
		photoFilePath
		nickName
		lastName
		middleName
		firstName
		remark
		mobilePhoneNumber
		weChatNumber
		addressCountry
		...
		success/fail/complete 回调函数
	回调结果：
		success 添加成功 errMsg： ok
		fail 用户取消操作 errMsg：fail cancel
		fail 调用失败，detail加上详情信息 errMsg: fail${detail}
	eg:
	contact: function(){
	    wx.addPhoneContact({
	      firstName: 'AAAAAA',
	      mobilePhoneNumber: '10086',
	      success: function(res){
	        console.log(res)
	      }
	    })
	}
2.wx.makePhoneCall - 拨打电话
	
	object参数说明:
		phoneNumber 需要拨打的电话号码 必填 String 
		success/fail/complete 回调函数
	eg：
	phone: function(){
		wx.makePhoneCall({
		  phoneNumber: '17629283930',
		  success: function(res){
		    console.log(res)
		  }
		})
	},
3.wx.scanCode - 调起客户端扫码界面，扫码成功后返回对应的结果
	
	object参数说明:
		onlyFromCamera 	是否只能从相机扫码，不允许从相册选择图片
		scanType 扫码类型，参数类型是数组，二维码是'qrCode'，一维码是'barCode'，DataMatrix是‘datamatrix’，pdf417是‘pdf417’
		success/fail/complete 回调函数
	success参数说明：
		result 所扫码的内容
		scanType 所扫码的类型
		charSet 所扫码的字符集
		path 当所扫的码为当前小程序的合法二维码时，会返回此字段，内容为二维码携带的 path
	eg：
	 scanCode: function(){
	    //允许相册和相机扫描
	    wx.scanCode({
	      success: function(res){
	        console.log(res)
	      }
	    })
	    //只允许相机
	    wx.scanCode({
	      onlyFromCamera: true,
	      success: function(res){
	        console.log(res)
	      }
	    })
	 },
4.wx.setClipboardData - 设置系统剪贴板的内容

	object参数说明:
		data 需要设置的剪贴板内容 String 必填
		success/fail/complete 回调函数
	eg：
	 clipBoard: function(){
	    //设置剪贴板的内容
	    wx.setClipboardData({
	      data: '设置剪贴板内容...',
	      success: function(){
	        console.log(res)
	      }
	    })
	 },
5.wx.getClipboardData - 获取系统剪贴板内容
	
	object参数说明:
		success/fail/complete 回调函数
	success参数说明：
		data 剪贴板内容 String
	eg：
	 clipBoard: function(){
	    //获取剪贴板的内容
	    wx.getClipboardData({
	      success: function(res){
	        console.log(res)
	      }
	    })
	  },

五、WiFi		
	
	支持搜索周边的WiFi，同时可以针对指定WiFi，传入密码发起连接;
	Wi-Fi 相关接口暂不可用 wx.canIUse 接口判断;
	Android 6.0 以上版本，在没有打开定位开关的时候会导致设备不能正常获取周边的 Wi-Fi 信息;
1.连接指定WiFi接口调用时序：
	
	Android + iOS(仅支持iOS11及以上版本):
		startWifi -> connectWifi -> onWifiConnected
2.连周边WiFi接口调用时序：
	
	Android:
		startWifi —> getWifiList —> onGetWifiList —> connectWifi —> onWifiConnected
	iOS(iOS 11.0及11.1版本因系统原因暂不支持):
		startWifi —> getWifiList —> onGetWifiList —> setWifiList —> onWifiConnected
3.wx.startWifi - 初始化WiFi模块
	
	object参数说明：
		success/fail/complete 回调函数
	eg:
	 wx.startWifi({
      success: function(res){
        console.log('success:' + res)
      },
      fail: function(res){
        console.log('fail:' + res)
      },
      complete: function(res){
        console.log('complete:' + res)
      }
    })
4.wx.stopWifi - 关闭WiFi模块	
	
	object参数说明：
		success/fail/complete 回调函数
	eg:
	 wx.stopWifi({
      success: function (res) {
        console.log('success:' + res)
      },
      fail: function (res) {
        console.log('fail:' + res)
      },
      complete: function (res) {
        console.log('complete:' + res)
      }
    })
5.wx.connectWifi - 连接WiFi
	
	若已知 Wi-Fi 信息，可以直接利用该接口连接。仅 Android 与 iOS 11 以上版本支持
	object参数说明：
		SSID wifi设备的ssid String 必填
		BSSID wifi设备的bssid String 必填
		password wifi设备的密码 String
		success/fail/complete 回调函数
	eg:
	wx.connectWifi({
      SSID: '',
      BSSID: '',
      success: function (res) {
        console.log('connectWifi - success:' + res)
      },
      fail: function (res) {
        console.log('connectWifi - fail:' + res)
      },
      complete: function (res) {
        console.log('connectWifi - complete:' + res)
      }
    })
6.wx.getWifiList - 获取WiFi列表
	
	在 onGetWifiList 注册的回调中返回 wifiList 数据;
	iOS 将跳转到系统的 Wi-Fi 界面，Android 不会跳转;
	iOS 11.0 及 iOS 11.1 两个版本因系统问题，该方法失效。但在 iOS 11.2 中已修复;
	object参数说明：
		success/fail/complete 回调函数
	eg:
	  wx.getWifiList({
      success: function (res) {
        console.log('success:' + res)
      },
      fail: function (res) {
        console.log('fail:' + res)
      },
      complete: function (res) {
        console.log('complete:' + res)
      }
    })
7.wx.onGetWifiList - 监听在获取到WiFi列表数据时的事件，在回调中将返回wifilist
	
	object参数说明：
		wifiList wifi列表数据
	wifi列表项说明:
		SSID wifi设备的ssid
		BSSID wifi设备的bssid
		secure wifi是否安全 Boolean
		signalStrength wifi信号强度 Number
	eg:
	wx.onGetWifiList(function(res){
      console.log('onGetWifiList:' + res)
    })
8.wx.setWifiList - iOS特有接口 在 onGetWifiList 回调后，利用接口设置 wifiList 中 AP 的相关信息

	该接口只能在 onGetWifiList 回调之后才能调用;
	此时客户端会挂起，等待小程序设置 Wi-Fi 信息，请务必尽快调用该接口，若无数据请传入一个空数组;
	有可能随着周边 Wi-Fi 列表的刷新，单个流程内收到多次带有存在重复的 Wi-Fi 列表的回调;
	object参数说明：
		wifiList 提供预设的wifi信息列表 必填 Array
	wifi列表项说明:
		SSID wifi设备的ssid
		BSSID wifi设备的bssid
		password wifi设备密码
	eg:
	wx.onGetWifiList(function(res) {
	  if (res.wifiList.length) {
	    wx.setWifiList({
	      wifiList: [{
	        SSID: res.wifiList[0].SSID,
	        BSSID: res.wifiList[0].BSSID,
	        password: '123456'
	      }]
	    })
	  } else {
	    wx.setWifiList({
	      wifiList: []
	    })
	  }
	})
	wx.getWifiList()
9.wx.onWifiConnected - 监听连接上WiFi的事件
	
	object参数说明：
		wifi  wifi信息 object
	wifi列表项说明:
		SSID wifi设备的ssid
		BSSID wifi设备的bssid
		secure wifi是否安全 Boolean
		signalStrength wifi信号强度 Number
	eg:
	wx.onWifiConnected(function(res){
      console.log('onWifiConnected:' + res)
    })
10.wx.getConnectedWifi - 获取已连接中的 Wi-Fi 信息
	
	object参数说明：
		success/fail/complete 回调函数
	success参数说明：
		wifi wifi信息
	eg:	
	wx.getConnectedWifi({
      success: function (res) {
        console.log('success:' + res)
      },
      fail: function (res) {
        console.log('fail:' + res)
      },
      complete: function (res) {
        console.log('complete:' + res)
      }
    })
11.errCode列表
	
|错误码|说明|备注|
|---|---|---|
|0	 | ok | 正常|
|	12000|nit init|未先调用startWifi接口|
|	12001|system not support|当前系统不支持相关能力|
|	12002|password error|Wi-Fi 密码错误|
|	12003|connection timeout|连接超时|
|	12004|duplicate request|重复连接 Wi-Fi|
|	12005|wifi not turned on|Android特有，未打开 Wi-Fi 开关|
|	12006|gps not turned on|Android特有，未打开 GPS 定位开关|
|	12007|user denuied|用户拒绝授权链接 Wi-Fi|
|	12008|invalid ssid|无效SSID|
|	12009|system config err|系统运营商配置拒绝连接 Wi-Fi|
|	12010|system internal error|系统其他错误，需要在errmsg打印具体的错误原因|
|	12011|weaoo in background| 应用在后台无法配置 Wi-Fi|
	
六、蓝牙		

	iOS 微信客户端 6.5.6 版本开始支持，Android 6.5.7 版本开始支持；
	目前不支持在开发者工具上进行调试，需要使用真机才能正常调用小程序蓝牙接口；	
1.wx.openBluetoothAdapter - 初始化小程序蓝牙模块

	生效周期为调用wx.openBluetoothAdapter至调用wx.closeBluetoothAdapter或小程序被销毁为止;
	在小程序蓝牙适配器模块生效期间，开发者可以正常调用下面的小程序API，并会收到蓝牙模块相关的on回调;
	
	在没有调用wx.openBluetoothAdapter的情况下调用小程序其它蓝牙模块相关API，API会返回错误，错误码为10000;
	在用户蓝牙开关未开启或者手机不支持蓝牙功能的情况下，调用wx.openBluetoothAdapter会返回错误，错误码为10001，表示手机蓝牙功能不可用；
	此时小程序蓝牙模块已经初始化完成，可通过wx.onBluetoothAdapterStateChange监听手机蓝牙状态的改变，也可以调用蓝牙模块的所有API;

	object参数说明:
		success/fail/complete 回调函数
	eg:
	wx.openBluetoothAdapter({
      success: function(res) {
        console.log(res)
      }
    })

2.wx.closeBluetoothAdapter - 关闭蓝牙模块
	
	使其进入未初始化状态。调用该方法将断开所有已建立的链接并释放系统资源；
	建议在使用小程序蓝牙流程后调用，与wx.openBluetoothAdapter成对调用；
	object参数说明:
		success/fail/complete 回调函数
	eg:
	wx.closeBluetoothAdapter({
      success: function(res) {
        console.log(res)
      },
    })
3.wx.getBluetoothAdapterState - 获取本机蓝牙适配器状态
	
	object参数说明:
		success/fail/complete 回调函数
	success参数说明:
		discovering 	是否正在搜索设备
		available		蓝牙适配器是否可用
		errMsg			成功：ok，错误：详细信息
	eg:
	 wx.getBluetoothAdapterState({
      success: function(res) {
        console.log(res)
      },
    })
4.wx.onBluetoothAdapterState - 监听蓝牙适配器状态变化事件

	callback参数说明:
		discovering 	是否正在搜索设备
		available		蓝牙适配器是否可用
	eg:
	wx.onBluetoothAdapterStateChange(function(res){
      console.log(res)
    })
5.wx.startBluetoothDeviceDiscovery - 开始搜寻附近的蓝牙外围设备
	
	该操作比较耗费系统资源，请在搜索并连接到设备后调用 stop 方法停止搜索;
	object参数说明:
		services  蓝牙设备主 service 的 uuid 列表
		allowDuplicatesKey 是否允许重复上报同一设备， 如果允许重复上报，则onDeviceFound 方法会多次上报同一设备，但是 RSSI 值会有不同
		interval 上报设备的间隔，默认为0，意思是找到新设备立即上报，否则根据传入的间隔上报
		success/fail/complete 回调函数
	services参数说明：
		某些蓝牙设备会广播自己的主 service 的 uuid。如果这里传入该数组，那么根据该 uuid 列表，只搜索发出广播包有这个主服务的蓝牙设备，建议主要通过该参数过滤掉周边不需要处理的其他蓝牙设备;
	success参数说明:
		errMsg			成功：ok，错误：详细信息
	eg:
	wx.startBluetoothDevicesDiscovery({
      success: function(res) {
        console.log(res)
      },
      complete: function(res){
        console.log(res)
      }
    })
6.wx.stopBluetoothDevicesDiscovery - 停止搜寻附近的蓝牙外围设备

	若已经找到需要的蓝牙设备并不需要继续搜索时，建议调用该接口停止蓝牙搜索;
	object参数说明:
		success/fail/complete 回调函数
	success参数说明:
		errMsg			成功：ok，错误：详细信息
	eg:
	 wx.stopBluetoothDevicesDiscovery({
      success: function (res) {
        console.log(res)
      },
      complete: function (res) {
        console.log(res)
      }
    })
7.wx.getBluetoothDevices - 获取在小程序蓝牙模块生效期间所有已发现的蓝牙设备，包括已经和本机处于连接状态的设备
	
	Mac系统可能无法获取advertisData及RSSI，请使用真机调试
	tip:
		 开发者工具和 Android 上获取到的deviceId为设备 MAC 地址，iOS 上则为设备 uuid。因此deviceId不能硬编码到代码中;
		 该接口获取到的设备列表为小程序蓝牙模块生效期间所有搜索到的蓝牙设备，若在蓝牙模块使用流程结束后未及时调用wx.closeBluetoothAdapter 释放资源，会存在调用该接口会返回之前的蓝牙使用流程中搜索到的蓝牙设备，可能设备已经不在用户身边，无法连接。
		 蓝牙设备在被搜索到时，系统返回的 name 字段一般为广播包中的LocalName字段中的设备名称，而如果与蓝牙设备建立连接，系统返回的 name 字段会改为从蓝牙设备上获取到的GattName。若需要动态改变设备名称并展示，建议使用localName字段;
	object参数说明:
		success/fail/complete 回调函数
	success参数说明:
		devices		uuid 对应的的已连接设备列表 Array
		errMsg			成功：ok，错误：详细信息
	device对象：
		name	String	蓝牙设备名称，某些设备可能没有
		deviceId	String	用于区分设备的 id
		RSSI	Number	当前蓝牙设备的信号强度
		advertisData	ArrayBuffer	当前蓝牙设备的广播数据段中的ManufacturerData数据段 （注意：vConsole 无法打印出 ArrayBuffer 类型数据）
		advertisServiceUUIDs	Array	当前蓝牙设备的广播数据段中的ServiceUUIDs数据段
		localName	String	当前蓝牙设备的广播数据段中的LocalName数据段
		serviceData	ArrayBuffer	当前蓝牙设备的广播数据段中的ServiceData数据段
	eg:
	wx.getBluetoothDevices({
      success: function (res) {
        console.log(res)
      },
      complete: function (res) {
        console.log(res)
      }
    })
8.wx.getConnectedBluetoothDevices - 根据 uuid 获取处于已连接状态的设备
	
	开发者工具和 Android 上获取到的deviceId为设备 MAC 地址，iOS 上则为设备 uuid。因此deviceId不能硬编码到代码中;
	object参数说明:
		service  蓝牙设备主 service 的 uuid 列表 必填 Array
		success/fail/complete 回调函数
	success参数说明:
		devices		搜索到的设备列表
		errMsg			成功：ok，错误：详细信息
	device对象:
		name		蓝牙设备名称，某些设备可能没有 String
		deviceId	用于区分设备的 id	String
	eg:
	wx.getConnectedBluetoothDevices({
      services: [''],
      success: function(res) {
        console.log(res)
      },
      complete: function(res){
        console.log(res)
      }
    })
9.wx.onBluetoothDeviceFound - 监听寻找到新设备的事件
	
	Mac系统可能无法获取advertisData及RSSI，请使用真机调试；
	开发者工具和 Android 上获取到的deviceId为设备 MAC 地址，iOS 上则为设备 uuid。因此deviceId不能硬编码到代码中；
	若在onBluetoothDeviceFound回调了某个设备，则此设备会添加到 wx.getBluetoothDevices 接口获取到的数组中；
	callback参数说明：
		device 新搜索到的设备列表
	eg:
	wx.onBluetoothDeviceFound(function(res){
      console.log(res)
    })
10.wx.createBLEConnection - 连接低功耗蓝牙设备
	
	若小程序在之前已有搜索过某个蓝牙设备，并成功建立链接，可直接传入之前搜索获取的deviceId直接尝试连接该设备，无需进行搜索操作;
	安卓手机上如果多次调用create创建连接，有可能导致系统持有同一设备多个连接的实例，导致调用close的时候并不能真正的断开与设备的连接。因此请保证尽量成对的调用create和close接口;
	蓝牙链接随时可能断开，建议监听 wx.onBLEConnectionStateChange 回调事件，当蓝牙设备断开时按需执行重连操作;
	若对未连接的设备或已断开连接的设备调用数据读写操作的接口，会返回10006错误，详见错误码，建议进行重连操作;
	object参数说明:
		deviceId  蓝牙设备 id，参考 getDevices 接口 必填 String
		success/fail/complete 回调函数
	success参数说明:
		errMsg			成功：ok，错误：详细信息
	eg:
	wx.createBLEConnection({
      deviceId: 'xxx',
      success: function(res) {
        console.log(res)
      },
    })
11.wx.closeBLEConnection - 断开与低功耗蓝牙设备的连接
	
	object参数说明:
		deviceId  蓝牙设备 id，参考 getDevices 接口 必填 String
		success/fail/complete 回调函数
	success参数说明:
		errMsg			成功：ok，错误：详细信息
	eg:
	wx.closeBLEConnection({
      deviceId: 'xxx',
      success: function(res) {
        console.log(res)
      },
    })
12.wx.getBLEDeviceServices - 获取蓝牙设备所有 service（服务）

	iOS平台上后续对特征值的read、write、notify，由于系统需要获取特征值实例，传入的 serviceId 与 characteristicId 必须由getBLEDeviceServices 与 getBLEDeviceCharacteristics 中获取到后才能使用。建议双平台统一在建立链接后先执行getBLEDeviceServices 与 getBLEDeviceCharacteristics 后再进行与蓝牙设备的数据交互
	object参数说明:
		deviceId  蓝牙设备 id，参考 getDevices 接口 必填 String
		success/fail/complete 回调函数
	success参数说明:
		services 		设备服务列表 Array
		errMsg			成功：ok，错误：详细信息
	service对象:
		uuid			蓝牙设备服务的 uuid
		isPrimary		该服务是否为主服务
	eg:
	wx.getBLEDeviceServices({
	  // 这里的 deviceId 需要已经通过 createBLEConnection 与对应设备建立链接 
	  deviceId: deviceId,
	  success: function (res) {
	    console.log('device services:', res.services)
	  }
	})
13.wx.getBLEDeviceCharacteristics - 获取蓝牙设备某个服务中的所有 characteristic（特征值）

	传入的serviceId需要在getBLEDeviceServices获取到
	iOS平台上后续对特征值的read、write、notify，由于系统需要获取特征值实例，传入的 serviceId 与 characteristicId 必须由getBLEDeviceServices 与 getBLEDeviceCharacteristics 中获取到后才能使用。建议双平台统一在建立链接后先执行getBLEDeviceServices 与 getBLEDeviceCharacteristics 后再进行与蓝牙设备的数据交互
	object参数说明:
		deviceId  蓝牙设备 id，参考 getDevices 接口 必填 String
		serviceId	蓝牙服务uuid 必填 String
		success/fail/complete 回调函数
	success参数说明:
		characteristics 		设备特征列表 Array
		errMsg			成功：ok，错误：详细信息
	characteristic对象:
		uuid			蓝牙设备特征的 uuid
		properties		该特征值支持的操作类型
	properties对象：
		read	Boolean	该特征值是否支持 read 操作
		write	Boolean	该特征值是否支持 write 操作
		notify	Boolean	该特征值是否支持 notify 操作
		indicate	Boolean	该特征值是否支持 indicate 操作
	eg:
	wx.getBLEDeviceCharacteristics({
	  // 这里的 deviceId 需要已经通过 createBLEConnection 与对应设备建立链接
	  deviceId: deviceId,
	  // 这里的 serviceId 需要在上面的 getBLEDeviceServices 接口中获取
	  serviceId: serviceId,
	  success: function (res) {
	    console.log('device getBLEDeviceCharacteristics:', res.characteristics)
	  }
	})
14.wx.readBLECharacteristicValue - 读取低功耗蓝牙设备的特征值的二进制数据值

	必须设备的特征值支持read才可以成功调用，具体参照 characteristic 的 properties 属性;
	并行调用多次读写接口存在读写失败的可能性;
	read接口读取到的信息需要在onBLECharacteristicValueChange方法注册的回调中获取;
	object参数说明:
		deviceId  蓝牙设备 id，参考 getDevices 接口 必填 String
		serviceId	蓝牙服务uuid 必填 String
		characteristicId 蓝牙特征值的uuid 必填 String
		success/fail/complete 回调函数
	success参数说明:
		errCode 		错误码
		errMsg			成功：ok，错误：详细信息
	eg:
	wx.readBLECharacteristicValue({
	  // 这里的 deviceId 需要已经通过 createBLEConnection 与对应设备建立链接  [**new**]
	  deviceId: deviceId,
	  // 这里的 serviceId 需要在上面的 getBLEDeviceServices 接口中获取
	  serviceId: serviceId,
	  // 这里的 characteristicId 需要在上面的 getBLEDeviceCharacteristics 接口中获取
	  characteristicId: characteristicId,
	  success: function (res) {
	    console.log('readBLECharacteristicValue:', res.errCode)
	  }
	})
15.wx.writeBLECharacteristicValue - 向低功耗蓝牙设备特征值中写入二进制数据

	必须设备的特征值支持write才可以成功调用，具体参照 characteristic 的 properties 属性;
	并行调用多次读写接口存在读写失败的可能性;
	tip:
		并行调用多次读写接口存在读写失败的可能性。
		小程序不会对写入数据包大小做限制，但系统与蓝牙设备会确定蓝牙4.0单次传输的数据大小，超过最大字节数后会发生写入错误，建议每次写入不超过20字节。
		安卓平台上，在调用notify成功后立即调用write接口，在部分机型上会发生 10008 系统错误
	bug：
		若单次写入数据过长，iOS平台上存在系统不会有任何回调的情况(包括错误回调)
	object参数说明:
		deviceId  蓝牙设备 id，参考 getDevices 接口 必填 String
		serviceId	蓝牙服务uuid 必填 String
		characteristicId 蓝牙特征值的uuid 必填 String
		value		蓝牙设备特征值对应的二进制值 必填 ArrayBuffer
		success/fail/complete 回调函数
	success参数说明:
		errCode 		错误码
		errMsg			成功：ok，错误：详细信息
	eg:
	// 向蓝牙设备发送一个0x00的16进制数据
	let buffer = new ArrayBuffer(1)
	let dataView = new DataView(buffer)
	dataView.setUint8(0, 0)
	
	wx.writeBLECharacteristicValue({
	  // 这里的 deviceId 需要在上面的 getBluetoothDevices 或 onBluetoothDeviceFound 接口中获取
	  deviceId: deviceId,
	  // 这里的 serviceId 需要在上面的 getBLEDeviceServices 接口中获取
	  serviceId: serviceId,
	  // 这里的 characteristicId 需要在上面的 getBLEDeviceCharacteristics 接口中获取
	  characteristicId: characteristicId,
	  // 这里的value是ArrayBuffer类型
	  value: buffer,
	  success: function (res) {
	    console.log('writeBLECharacteristicValue success', res.errMsg)
	  }
	})
16.wx.notifyBLECharacteristicValueChange - 启用低功耗蓝牙设备特征值变化时的 notify 功能，订阅特征值

	必须设备的特征值支持notify或者indicate才可以成功调用，具体参照 characteristic 的 properties 属性；
	必须先启用notify才能监听到设备 characteristicValueChange 事件；
	tips:
		订阅操作成功后需要设备主动更新特征值的value，才会触发 wx.onBLECharacteristicValueChange 回调;
		安卓平台上，在调用notify成功后立即调用write接口，在部分机型上会发生 10008 系统错误;
	object参数说明:
		deviceId  蓝牙设备 id，参考 getDevices 接口 必填 String
		serviceId	蓝牙服务uuid 必填 String
		characteristicId 蓝牙特征值的uuid 必填 String
		state		true: 启用 notify; false: 停用 notify 必填
		success/fail/complete 回调函数
	success参数说明:
		errMsg			成功：ok，错误：详细信息
	eg:
	wx.notifyBLECharacteristicValueChange({
	  state: true, // 启用 notify 功能
	  // 这里的 deviceId 需要已经通过 createBLEConnection 与对应设备建立链接  
	  deviceId: deviceId,
	  // 这里的 serviceId 需要在上面的 getBLEDeviceServices 接口中获取
	  serviceId: serviceId,
	  // 这里的 characteristicId 需要在上面的 getBLEDeviceCharacteristics 接口中获取
	  characteristicId: characteristicId,
	  success: function (res) {
	    console.log('notifyBLECharacteristicValueChange success', res.errMsg)
	  }
	})
17.wx.onBLEConnectionStateChange - 监听低功耗蓝牙连接状态的改变事件，包括开发者主动连接或断开连接，设备丢失，连接异常断开等等
	
	callback参数说明:
		deviceId  蓝牙设备 id，参考 getDevices 
		connected	连接目前的状态
	eg:
	wx.onBLEConnectionStateChange(function(res) {
	  // 该方法回调中可以用于处理连接意外断开等异常情况
	  console.log(`device ${res.deviceId} state has changed, connected: ${res.connected}`)
	})
18.wx.onBLECharacteristicValueChange - 监听低功耗蓝牙设备的特征值变化

	必须先启用notify接口才能接收到设备推送的notification
	callback参数说明:
		deviceId  蓝牙设备 id，参考 getDevices 
		serviceId	特征值所属服务 uuid
		characteristicId 特征值的uuid 
		value		特征值最新的值 （注意：vConsole 无法打印出 ArrayBuffer 类型数据）
	eg:
	// ArrayBuffer转16进度字符串示例
	function ab2hex(buffer) {
	  var hexArr = Array.prototype.map.call(
	    new Uint8Array(buffer),
	    function(bit) {
	      return ('00' + bit.toString(16)).slice(-2)
	    }
	  )
	  return hexArr.join('');
	}
	wx.onBLECharacteristicValueChange(function(res) {
	  console.log(`characteristic ${res.characteristicId} has changed, now is ${res.value}`)
	  console.log(ab2hext(res.value))
	})

19.错误码
	
	
|错误码|	说明|	备注|
|---|---|---|---|
|0|	ok	|正常|
|10000	|not init	|未初始化蓝牙适配器|
|10001	|not available	|当前蓝牙适配器不可用|
|10002	|no device	|没有找到指定设备|
|10003	|connection fail	|连接失败|
|10004	|no service	|没有找到指定服务|
|10005	|no characteristic	|没有找到指定特征值|
|10006	|no connection	|当前连接已断开|
|10007	|property not support	|当前特征值不支持此操作|
|10008	|system error	|其余所有系统上报的异常|
|10009|	system not support	|Android 系统特有，系统版本低于 4.3 不支持BLE|
七、NFC		

	暂仅支持 HCE（基于主机的卡模拟）模式，即将安卓手机模拟成实体智能卡;
	适用机型：支持 NFC 功能，且系统版本为Android5.0及以上的手机;
	适用卡范围：符合ISO 14443-4标准的CPU卡;
1.wx.getHCEState - 判断当前设备是否支持 HCE 能力

	object参数说明：
		success/fail/complete 回调函数
	success参数列表:
		errMsg  错误信息	String
		errCode 错误代码 	Number
	eg:
	wx.getHCEState({
      success: function (res) {
        console.log('success:' + res)
      },
      fail: function (res) {
        console.log('fail:' + res)
      },
      complete: function (res) {
        console.log('complete:' + res)
      }
    })
2.wx.startHCE - 初始化 NFC 模块
	
	object参数说明：
		aid_list	需要注册到系统的 AID 列表，每个 AID 为 String 类型 必填 Array
		success/fail/complete 回调函数
	success参数列表:
		errMsg  错误信息	String
		errCode 错误代码 	Number
	eg:
	wx.startHCE({
      aid_list: [''],
      success: function (res) {
        console.log('success:' + res)
      },
      fail: function (res) {
        console.log('fail:' + res)
      },
      complete: function (res) {
        console.log('complete:' + res)
      }
    })
3.wx.stopHCE - 关闭 NFC 模块,仅在安卓系统下有效;

	object参数说明：
		success/fail/complete 回调函数
	success参数列表:
		errMsg  错误信息	String
		errCode 错误代码 	Number
	eg:
	wx.stopHCE({
      success: function (res) {
        console.log('success:' + res)
      },
      fail: function (res) {
        console.log('fail:' + res)
      },
      complete: function (res) {
        console.log('complete:' + res)
      }
    })
4.wx.onHCEMessage - 监听 NFC 设备的消息回调，并在回调中处理。返回参数中 messageType 表示消息类型，目前有如下值：

	消息为HCE Apdu Command类型，小程序需对此指令进行处理，并调用 sendHCEMessage 接口返回处理指令；
	消息为设备离场事件;
	callback参数说明:
		messageType 消息类型 Number
		data 	客户端接收到 NFC 设备的指令，此参数当且仅当 messageType=1 时有效 ArrayBuffer
		reason 此参数当且仅当 messageType=2 时有效 Number
	eg:
	wx.onHCEMessage(function(res){
      console.log(res)
    })
5.wx.sendHCEMessage - 发送 NFC 消息。仅在安卓系统下有效
	
	object参数说明：
		data 二进制数据 必填 ArrayBuffer
		success/fail/complete 回调函数
	success参数列表:
		errMsg  错误信息	String
		errCode 错误代码 	Number
	eg:
	const buffer = new ArrayBuffer(1)
	const dataView = new DataView(buffer)
	dataView.setUint8(0, 0)
	
	wx.startHCE({
	  success: function(res) {
	    wx.onHCEMessage(function(res) {
	      if (res.messageType === 1) {
	        wx.sendHCEMessage({data: buffer})
	      }
	    })
	  }
	})
6.errCode列表

|错误码	|说明|
|---|---|
|0	|Ok|
|13000	|当前设备不支持 NFC|
|13001	|当前设备支持 NFC，但系统NFC开关未开启|
|13002	|当前设备支持 NFC，但不支持HCE|
|13003	|AID 列表参数格式错误|
|13004	|未设置微信为默认NFC支付应用|
|13005	|返回的指令不合法|
|13006	|注册 AID 失败|

八、罗盘、加速度计		
1.wx.startCompass - 开始监听罗盘数据
	
	object参数说明：
		success/fail/complete 回调函数
	eg:
	wx.startCompass({
      success: function (res) {
        console.log(res)
      }
    })
2.wx.stopCompass - 停止监听罗盘数据
	
	object参数说明：
		success/fail/complete 回调函数
	eg:
	wx.stopCompass({
      success: function(res){
        console.log(res)
      }
    })
3.wx.onCompassChange - 监听罗盘数据
	
	频率：5次/秒，接口调用后会自动开始监听，可使用wx.stopCompass停止监听;
	callback参数说明：
		direction 面对的方向度数 Number
	eg:
	wx.onCompassChange(function (res) {
      console.log(res)
    })
4.wx.startAccelerometer - 开始监听加速度数据
	
	object参数说明：
		success/fail/complete 回调函数
	eg:
	wx.startAccelerometer({
        success: function (res) {
          console.log(res)
        }
      }
5.wx.stopAccelerometer - 停止监听加速度数据
	
	object参数说明：
		success/fail/complete 回调函数
	eg:
	wx.stopAccelerometer({
        success: function (res) {
          console.log(res)
        }
      })
6.wx.onAccelerometerChange - 监听加速度数据
	
	频率：5次/秒，接口调用后会自动开始监听，可使用 wx.stopAccelerometer 停止监听;
	callback参数说明:
		x X轴 Number
		y Y轴 Number
		z Z轴 Number
	eg：
	  wx.onAccelerometerChange(function(res){
        console.log(res)
      })  
九、iBeacon	
1.wx.startBeaconDiscovery	- 开始搜索附近的iBeacon设备
	
	object参数说明:
		uuids iBeacon设备广播的uuids 必填 StringArray
		success/fail/complete 回调函数
	success参数说明：
		errMsg 调用结果 String
	eg:
	wx.startBeaconDiscovery({
      uuids: ["xxxxxxxxxxxxx"],
      success: function(res){
        console.log(res)
      },
      complete: function(res){
        console.log(res)
      }
    })
2.wx.stopBeaconDiscovery - 停止搜索附近的iBeacon设备
	
	object参数说明:
		success/fail/complete 回调函数
	success参数说明：
		errMsg 调用结果 String
	eg:
	wx.stopBeaconDiscovery({
      success: function(res){
        console.log(res)
      },
      complete: function(res){
        console.log(res)
      }
    })
3.wx.getBeacons - 获取所有已搜索到的iBeacon设备
	
	object参数说明:
		success/fail/complete 回调函数
	success参数说明：
		beacons iBeacon设备列表 objectArray
		errMsg 调用结果 String
	iBeacon结构：
		uuid		iBeacon设备广播的uuid
		major	iBeacon设备的主id
		minor	iBeacon设备的次id
		proximity	表示设备距离的枚举值
		accuracy	iBeacon设备的距离
		rssi		表示设备的信号强度
	eg:
	wx.getBeacons({
      success: function (res) {
        console.log('success:' + res)
      },
      fail: function (res) {
        console.log('fail:' + res)
      },
      complete: function (res) {
        console.log('complete:' + res)
      }
    })
4.wx.onBeaconUpdate - 监听 iBeacon 设备的更新事件
	
	callback参数说明:
		beacons 当前搜寻到的所有 iBeacon 设备列表 objectArray
	iBeacon结构
		如上所示
	eg:
	wx.onBeaconUpdate(function(res){
      console.log(res)
    })
5.wx.onBeaconServiceChange - 监听 iBeacon 服务的状态变化
	
	callback参数说明:
		available  服务目前是否可用
		discovering 目前是否处于搜索状态
	eg:
	wx.onBeaconServiceChange(function(res){
      console.log(res)
    })	
6.错误码列表

|错误码|说明|备注|
|---|---|---|
|0	 | ok | 正常|
|	11000| unsupport |系统或设备不支持|
|	11001|bluetooth service unavailable|蓝牙服务不可用|
|	11002|location service unavailable|位置服务不可用|
|	11003|already star|已经开始搜索|

参考资料：	
1.[设备](https://developers.weixin.qq.com/miniprogram/dev/api/systeminfo.html)	
2.[WiFi文档](https://mp.weixin.qq.com/wiki?t=resource/res_main&id=215135855720FBA0)