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

六、蓝牙

七、NFC

八、罗盘

九、iBeacon

十、加速度计



参考资料：	
1.[设备](https://developers.weixin.qq.com/miniprogram/dev/api/systeminfo.html)