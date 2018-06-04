---
title: 微信小程序-API-开发接口
date: 2018-06-04 10:08:37
tags: 微信小程序
categories: 小程序
---

一、登录	

	通过登录能方便的获取微信提供的用户身份识别标志，快速建立小程序的用户体系;
登录流程时许图:		
![api-login](api-login.jpg)	
	
	调用wx.login()获取临时登录凭证code,并回传到开发者服务器;
	开发者服务器以code换取用户唯一标识openid和会话密钥session_key;
	之后 开发者服务器可以根据用户标识来生成自定义登录态，用于后续业务逻辑中前后端交互时识别用户身份。
1.wx.login() - 获取临时登录凭证(code)	
	
	object参数说明:
		timeout 超时时间 ms Number
		success/fail/complete 回调函数
	success参数说明:
		errMsg 调用结果
		code	用户登录凭证(有效期5分钟),开发者需要在开发者服务器后台调用API，使用code换取openid和session_key等信息
	eg:
	wx.login({
      success: function(res){
        console.log(res)
      }
    })
2.登录凭证校验	
	
	会话密钥session_key是对用户数据进行加密签名的密钥。为了应用自身的数据安全，开发者服务器不应该把会话密钥下发到小程序，也不应该对外提供这个密钥;
	UnionID只满足一定条件的情况下返回;
	临时登录凭证code只能使用一次;
	bug:
		iOS/Android 6.3.30，在 App.onLaunch 调用 wx.login 会出现异常;
	url:
		https://api.weixin.qq.com/sns/jscode2session?appid=APPID&secret=SECRET&js_code=JSCODE&grant_type=authorization_code
	请求参数： 均为必填
		appid 			小程序唯一标识 
		secret  		小程序的 app secret
		js_code 		登录时获取的 code
		grant_type 	填写为 authorization_code
	返回参数:
		不满足UnionID下发条件：
			openid		用户唯一标识
			session_key	会话密钥
		满足UnionID下发条件：
			openid
			session_key
			unionid		用户在开放平台的唯一标识符
	eg:返回说明
	//正常返回的JSON数据包
	{
	      "openid": "OPENID",
	      "session_key": "SESSIONKEY",
	}
	
	//满足UnionID返回条件时，返回的JSON数据包
	{
	    "openid": "OPENID",
	    "session_key": "SESSIONKEY",
	    "unionid": "UNIONID"
	}
	//错误时返回JSON数据包(示例为Code无效)
	{
	    "errcode": 40029,
	    "errmsg": "invalid code"
	}	
3.wx.checkSession - 检查用户当前session_key是否有效

	object参数：
		success/fail/complete 回调函数
	eg:
	wx.checkSession({
      success: function(res){
      //session_key 未过期，并且在本生命周期一直有效
        console.log(res)
      },
      fail: function (res){
      // session_key 已经失效，需要重新执行登录流程
        console.log(res)
      }
    })
4.签名加密		
用户数据的签名验证和加解密		
![signature](signature.jpg)		
	
	签名校验以及数据加解密涉及用户的会话密钥session_key;
	开发者先通过wx.login登录流程获取会话密钥session_key并保存在服务器;
	开发者不应该把session_key传到小程序客户端等服务器外的环境;
数据签名校验		
	
	通过调用接口（如 wx.getUserInfo）获取数据时，接口会同时返回 rawData、signature，其中 signature = sha1( rawData + session_key );
	开发者将 signature、rawData 发送到开发者服务器进行校验。服务器利用用户对应的 session_key 使用相同的算法计算出签名 signature2 ，比对 signature 与 signature2 即可校验数据的完整性;
加密数据解密算法		
	
	接口的明文内容不包含敏感数据(如：openId和unionId等),要获取敏感数据需要对返回数据(加密数据)进行对称解密：
		对称解密使用的算法为 AES-128-CBC，数据采用PKCS#7填充。
		对称解密的目标密文为 Base64_Decode(encryptedData)。
		对称解密秘钥 aeskey = Base64_Decode(session_key), aeskey 是16字节。
		对称解密算法初始向量 为Base64_Decode(iv)，其中iv由数据接口返回
	为了应用校验数据的有效性，会在敏感数据加上数据水印(watermark)
	watermark参数说明:
		appid	敏感数据归属appid，开发者可校验此参数与自身appid是否一致 String
		timestamp 敏感数据获取的时间戳, 开发者可以用于数据时效性校验 DateInt
会话密钥session_key有效性	

	遇到session_key不正确而签名失败或解密失败:
		wx.login()调用时，用户的session_key会被更新而致使旧session_key失效。开发者应该在明确需要重新登录时才调用wx.login()，及时通过登录凭证校验接口更新服务器存储的session_key;
		微信不会把session_key的有效期告知开发者。我们会根据用户使用小程序的行为对session_key进行续期。用户越频繁使用小程序，session_key有效期越长;
		开发者在session_key失效时，可以通过重新执行登录流程获取有效的session_key。使用接口wx.checkSession()可以校验session_key是否有效，从而避免小程序反复执行登录流程;
		当开发者在实现自定义登录态时，可以考虑以session_key有效期作为自身登录态有效期，也可以实现自定义的时效性策略;
二、授权		
部分接口需要获得用户授权同意才能调用：	
	
	如果用户未接受或拒绝过此权限，会弹窗询问用户，用户点击同意后方可调用接口；
	如果用户已授权，可以直接调用接口；
	如果用户已拒绝授权，则短期内不会出现弹窗，而是直接进入接口 fail 回调；
	兼容用户拒绝授权的场景；
1.wx.getSetting - 获取授权信息
	
	wx.getSetting({
      success: function(res){
        console.log(res)
      }
    })
2.wx.openSetting - 打开设置界面
	
	wx.openSetting({
      success: function (res) {
        console.log(res)
      }
    })
3.wx.authorize - 提前发起授权请求

	wx.authorize({scope: "scope.userInfo"})，无法弹出授权窗口，请使用 <button open-type="getUserInfo"></button>
	scope列表:
		scope.userInfo   用户信息 wx.getUserInfo
		scope.userLocation 地理位置 wx.getLocation, wx.chooseLocation
		scope.address 通讯地址  wx.chooseAddress
		scope.invoiceTitle 发票抬头 wx.chooseInvoiceTitle
		scope.werun  微信运动步数 wx.getWeRunData
		scope.record 录音功能 wx.startRecord
		scope.writePhotoalbum 保存到相册 wx.saveImageToPhotosAlbum, wx.saveVideoToPhotosAlbum
		scope.camera 摄像头 <camera />
	eg:
	wx.authorize({
      scope: '',
      success: function(res){
        console.log(res)
      }
    })

三、用户信息		
1.wx.getUserInfo - 获取用户信息

2.getPhoneNumber - 获取微信用户绑定的手机号	

3.UnionID机制说明


四、微信支付


五、模版消息



参考资料：		
1.[开发接口](https://developers.weixin.qq.com/miniprogram/dev/api/api-login.html)		
2.[UnionID机制说明](https://developers.weixin.qq.com/miniprogram/dev/api/unionID.html)		
3.[]()		
4.[]()		



