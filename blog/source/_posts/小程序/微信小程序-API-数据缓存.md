---
title: 微信小程序-API-数据缓存
date: 2018-05-18 09:07:55
tags: 微信小程序
categories: 小程序
---

一、缓存简介	
1.大小：同一个微信号，同一个小程序storage上限10M。  

	如果用户存储空间不足，会清空最近最久未使用的小程序的本地缓存;
	不建议将关键信息全部存在本地缓存，以防存储空间不足或用户更换设备的情况;
2.安全:本地缓存localStorage以用户纬度隔离，同一台设备上，A用户无法读取B用户数据。		
3.存储方式:
	
	wx.setStorage(wx.setStorageSync) 写入
	wx.getStorage(wx.getStorageSync) 读取指定信息
	wx.getStorageInfo(wx.getStorageSync) 读取storage信息
	wx.clearStorage(wx.clearStorageSync) 清除
	wx.removeStorage(wx.removeStorageSync)清除指定信息
二、写入	
1.同步 wx.setStorageSync(key,data)      	

	将数据 同步 存储在本地缓存中指定的key中，会覆盖原来该key对应内容;
2.异步 wx.setStorage(object)		
	
	将数据 异步 存储在本地缓存中指定的key中，会覆盖原来该key对应内容;
3.参数说明

|参数|类型|必填|描述|
|---|---|---|---|
|key|String|是|本地缓存中指定的key|
|data|Object/String|是|存储的内容|
|success|Function|否|调用成功的回调函数|
|fail|Function|否|调用失败的回调函数|
|complete|Function|否|调用结束的回调函数(成功、失败都执行)|	
4.eg

	setStorageAction: function () {
	    var key = this.data.key
	    var data = this.data.data
	    if (key.length === 0) {//key不存在，弹框提示
	      this.setData({
	        key: key,
	        data: data,
	        'dialog.hidden': false,
	        'dialog.title': '保存数据失败',
	        'dialog.content': 'key 不能为空'
	      })
	    } else {
	      //同步 wx.setStorageSync
	      wx.setStorageSync(key, data)
	      this.setData({
	        key: key,
	        data: data,//String或object
	        'dialog.hidden': false,
	        'dialog.title': '存储数据成功'
	      })
	      
	      异步 - wx.setStorage
	      wx.setStorage({
	        key: key,
	        data: data,
	        success: function(res){
	          console.log('success:' + res.errMsg)
	        },
	        fail: function(res){
	          console.log("fail:" + res)
	        },
	        complete: function(res){
	          console.log('complete:'+ res)
	        }
	      })
	    }
	}

三、读取		
1.同步wx.getStorageSync(key)				

	从本地缓存 同步 获取指定key对应的内容；
2.异步wx.getStorage(object)		

	从本地缓存 异步 获取指定key对应的内容；
3.同步wx.getStorageInfosync
	
	同步 获取当前stroage的相关信息；
4.异步wx.getStorageInfo(object)
	
	异步 获取当前storage的相关信息；
5.参数说明

|参数|类型|必填|描述|
|---|---|---|---|
|wx.getStorage||||
|key|String|是|本地缓存中的指定的 key|
|success|Function|是| 调用成功的回调函数,res = {data: key对应的内容}|
|fail| Function |否|调用失败的回调函数|
|complete| Function |否|调用结束的回调函数（调用成功、失败都会执行）|
|success返回参数描述||||
|data| String ||key对应的内容|
|||||
|wx.getStorageSync||||
|key| String |是|本地缓存中的指定key|
|||||
|wx.getStorageInfo||||
|success| Function |是|调用成功的回调函数|
|fail| Function |否|调用失败的回调函数|
|complete| Function |否|调用结束的回调函数(调用成功、失败都会执行)|
|success返回参数描述||||
|keys| String  Array||当前storage中所有key|
|currentSize|Number||当前占用空间的大小,kb|
|limitSize|Number||限制空间的大小,kb|

6.eg		

	getStorageAction: function () {
	    var key = this.data.key,
	        data = this.data.data
	    var storageData
	
	    if (key.length === 0) {
	      this.setData({
	        key: key,
	        data: data,
	        'dialog.hidden': false,
	        'dialog.title': '读取数据失败',
	        'dialog.content': 'key 不能为空'
	      })
	    } else {
	      //异步获取指定key对应内容
	      wx.getStorage({
	        key: key,
	        success: function(res) {
	          console.log(res.data)
	        },
	      }),
	 	   //同步获取指定key对应内容
	      storageData = wx.getStorageSync(key)
	      if (storageData === "") {
	        this.setData({
	          key: key,
	          data: data,
	          'dialog.hidden': false,
	          'dialog.title': '读取数据失败',
	          'dialog.content': '找不到 key 对应的数据'
	        })
	      } else {
	        this.setData({
	          key: key,
	          data: data,
	          'dialog.hidden': false,
	          'dialog.title': '读取数据成功',
	          'dialog.content': "data: '"+ storageData + "'"
	        })
	      }
	      //同步获取storage内容
	      try {
	        var res = wx.getStorageInfoSync()
	        console.log('getStorageInfoSync:' + res.keys)
	        console.log('getStorageInfoSync:' + res.currentSize)
	        console.log('getStorageInfoSync:' + res.limitSize)
	      } catch (e) {
	        // Do something when catch error
	        console.log('error')
	      }
	      //异步获取storage内容
	      wx.getStorageInfo({
	        success: function(res) {
	          console.log('getStorageInfo:' + res.keys)
	          console.log('getStorageInfo:' + res.currentSize)
	          console.log('getStorageInfo:' + res.limitSize)
	        },
	      })
	    }
	}
四、清除		
1.同步wx.removeStorageSync(key)

	从本地缓存中 同步 移除指定 key 
2.异步wx.removeStorage(key)
	
	从本地缓存中 异步 移除指定 key 
3.同步wx.clearStorageSync
	
	同步 清理本地数据缓存；
4.异步wx.clearStorage
	
	异步 清理本地数据缓存；
5.参数说明

|参数|类型|必填|描述|
|---|---|---|---|
|key|String|是|本地缓存中的指定的 key|
|success|Function|是|调用成功的回调函数|
|fail| Function |否|调用失败的回调函数|
|complete| Function |否|调用结束的回调函数（调用成功、失败都会执行）|
6.eg

	clearStorageAction: function () {
		var key = this.data.key,
	        data = this.data.data
	        
		//同步 清理本地数据缓存
	    wx.clearStorageSync()
	    this.setData({
	      key: '',
	      data: '',
	      'dialog.hidden': false,
	      'dialog.title': '清除数据成功',
	      'dialog.content': ''
	    })
	    //异步 清理本地数据缓存
	    wx.clearStorage()
	    
	    //从本地缓存中异步移除指定 key
	    wx.removeStorage({
		  key: key,
		  success: function(res) {
		    console.log(res.data)
		  } 
		})
		//从本地缓存中同步移除指定 key
		try {
		  wx.removeStorageSync('key')
		} catch (e) {
		  // Do something when catch error
		}
	}
参考资料:		
1.[API-数据缓存](https://developers.weixin.qq.com/miniprogram/dev/api/data.html)		