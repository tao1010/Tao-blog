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
	
	此接口有调整，使用该接口将不再出现授权弹窗，请使用 <button open-type="getUserInfo"></button> 引导用户主动进行授权操作;
	当用户未授权过，调用该接口将直接报错;
	当用户授权过，可以使用该接口获取用户信息;
	object参数说明:
		withCredentials 是否带上登录态信息 Boolean
		lang	指定返回用户信息的语言，zh_CN简体中文，zh_TW繁体中文，en英文，默认en
		timeout	超时时间 单位ms
		success/fail/complete 回调函数
		
		当 withCredentials 为 true 时，要求此前有调用过 wx.login 且登录态尚未过期，此时返回的数据会包含 encryptedData, iv 等敏感信息；
		当 withCredentials 为 false 时，不要求有登录态，返回的数据不包含 encryptedData, iv 等敏感信息；
		
	success参数说明：
		userInfo 	用户信息对象，不包含openid等敏感信息
		rawData	不包括敏感信息的原始数据字符串，用于计算签名
		signature	使用sha1(rawData+sessionkey)得到的字符串，用于校验用户信息
		encryptedData	 包括敏感数据在内的完整用户信息的加密数据
		iv 			加密算法的初始向量
	userInfo参数说明：
		nickName	用户昵称
		avatarUrl	用户头像 最后一个数值代表正方形头像大小（有0、46、64、96、132数值可选，0代表640*640正方形头像），用户没有头像时该项为空。若用户更换头像，原有头像URL将失效
		gender	用户性别 1-男 2-女 0-未知
		city	用户所在城市
		province	用户所省份
		country	用户所在国家
		language 用户语言 
	eg:
	wx.getSetting({
      success: function(res){
        console.log(res)
        if(res.authSetting['scope.useInfo']){
          wx.getUserInfo({
            success: function (res){
              console.log(res.userInfo)
            }
          })
        }
      }
    })
		
2.getPhoneNumber - 获取微信用户绑定的手机号	

	需先调wx.login接口;
	需要用户主动触发才能发起获取手机号接口，需用<button>组件的点击来触发;
	该接口针对非个人开发者，且完成了认证的小程序开放;
	使用方法：
		在button组件中设置open-type值，bindgetphonenumber点击事件，通过点击事件的回调获取微信服务器返回的加密数据，然后在第三方服务器结合session_key、app_id进行解密获取手机号码;
		<button open-type="getPhoneNumber" bindgetphonenumber="getPhoneNumber"> </button>
		
		 getPhoneNumber: function(e) { 
	        console.log(e.detail.errMsg) 
	        console.log(e.detail.iv) 
	        console.log(e.detail.encryptedData) 
	    }
	返回参数：
   		encryptedData 包括敏感数据在内的完整用户信息的加密数据
   			phoneNumber	用户绑定的手机号（国外手机号有区号）
   			purePhoneNumber	没有区号的手机号
   			countryCode	区号
   		iv	加密算法的初始向量
	tips:
	  在回调中调用 wx.login 登录，可能会刷新登录态。此时服务器使用 code 换取的 sessionKey 不是加密时使用的 sessionKey，导致解密失败。
	  建议开发者提前进行 login；或者在回调中先使用 checkSession 进行登录态检查，避免 login 刷新登录态 	  
3.UnionID机制说明
	
	同一用户，对同一个微信开放平台下的不同应用，unionid是相同的;
	获取途径：（绑定开发者账号）
		wx.getUserInfo,从解密数据中获取Unionid;（需授权）
		wx.login获取用户Unionid，无需授权;（开发者帐号下存在同主体的公众号，并且该用户已经关注了该公众号）
		wx.login获取用户Unionid，无需授权;（开发者帐号下存在同主体的公众号或移动应用，并且该用户已经授权登录过该公众号或移动应用）

四、微信支付微信运动
1.wx.requestPayment - 发起微信支付
	
	6.5.2 及之前版本中，用户取消支付不会触发 fail 回调，只会触发 complete 回调，回调 errMsg 为 'requestPayment:cancel'
	object参数说明:
		timeStamp 时间戳从1970年1月1日00:00:00至今的秒数,即当前的时间
		nonceStr 随机字符串，长度为32个字符以下
		package 统一下单接口返回的 prepay_id 参数值，提交格式如：prepay_id=*
		signType 签名算法，暂支持 MD5
		paySign 签名
		success/fail/complete 回调函数
	eg:
	wx.requestPayment({
      timeStamp: '',
      nonceStr: '',
      package: '',
      signType: '',
      paySign: '',
      success: function(res){
        console.log(res)
      },
      fail: function(res){
        console.log(res)
      },
      complete: function(res){
        console.log(res)
      }
    })
2.wx.getWeRunData - 获取用户过去三十天微信运动步数
	
	需要先调用wx.login接口;
	需用用户授权scope.werun;
	object参数说明:
		timeout	超时时间 单位ms
		success/fail/complete 回调函数
	success参数说明:
		errMsg	调用结果
		encryptedData	 包括敏感数据在内的完整用户信息的加密数据
			stepInfoList 用户过去三十天的微信运动步数 objectArray
				timestamp 时间戳，表示数据对应的时间 Number
				step		微信运动步数 Number
		iv	加密算法的初始向量
	eg:
	wx.getSetting({
      success: function (res) {
        
        if (res.authSetting['scope.werun']) {
          console.log(res)
          wx.getWeRunData({
            success: function (res) {
              console.log(res)
            }
          })
        }else{
          console.log('未授权')
          wx.authorize({
            scope: 'scope.werun',
            success:function(res) {
              wx.run()
            }
          })
        }
      }
    })
五、转发
1.onShareAppMessage - 转发信息
	
	在.js文件的Page中定义函数onShareAppMessage;
	只有定义了此事件处理函数，右上角菜单才会显示 “转发” 按钮；
	用户点击转发按钮的时候会调用；
	此事件需要 return 一个 Object，用于自定义转发内容；
	options参数说明：
		form	转发事件来源 button：页面内转发按钮；menu：右上角转发菜单
		target 如果 from 值是 button，则 target 是触发这次转发事件的 button，否则为 undefined
	自定义转发字段
		title	转发标题，默认： 当前小程序名称
		path	转发路径，默认：当前页面 path ，必须是以 / 开头的完整路径
		imageUrl 自定义图片路径，可以是本地文件路径、代码包文件路径或者网络图片路径，支持PNG及JPG，不传入 imageUrl 则使用默认截图。显示图片长宽比是 5:4
	eg：
	onShareAppMessage: function(){
	    if (res.from === 'menu') {
	      // 来自页面内转发按钮
	      console.log(res.target)
	    }
	    return {
	      title: '自定义转发标题',
	      path: '/pages/login/login',
	      imageUrl: '/pages/images/bd.png'
	    }
  	}
2.wx.showShareMenu - 显示当前页面的转发按钮
	
	object参数说明:
		withShareTicket 是否使用带shareTicket的转发
		success/fail/complete 回调函数
	eg：
	wx.showShareMenu({
      withShareTicket: true,
      success(){
        console.log('success')
      }
    })
3.wx.hidenShareMenu - 隐藏转发按钮
	
	object参数说明:
			success/fail/complete 回调函数
	eg:
	wx.hideShareMenu({
      success(){
        console.log('success')
      }
    })
4.wx.updateShareMenu - 更新转发属性

	object参数说明:
		withShareTicket 是否使用带shareTicket的转发
		success/fail/complete 回调函数
	eg：
  	wx.updateShareMenu({
	  withShareTicket: true,
	  success() {
	  		console.log('success')
	  }
	})
 5.wx.getShareInfo - 获取转发详情信息
 
 	object参数说明:
 		shareTicket 必填 String
 		timeout	超时时间
 		success/fail/complete 回调函数
 	返回参数说明:
 		errMsg	错误信息
 		encrytedData 包括敏感数据在内的完整转发信息的加密数据
 			openGId 群对当前小程序的唯一 ID
 		iv 加密算法的初始向量
 	eg：
 	wx.getShareInfo({
          shareTicket: '',
          success: function(res){
            console.log(res)
          }
        })
6.获取更多转发信息
 
 	通常开发者希望转发出去的小程序被二次打开的时候能够获取到一些信息，例如群的标识。
 	通过调用 wx.showShareMenu 并且设置 withShareTicket 为 true ，当用户将小程序转发到任一群聊之后，此转发卡片在群聊中被其他用户打开时，可以在 App.onLaunch() 或 App.onShow 获取到一个 shareTicket。
 	通过调用 wx.getShareInfo() 接口传入此 shareTicket 可以获取到转发信息
7.页面内转发
	
	通过给 button 组件设置属性 open-type="share"，可以在用户点击按钮后触发 Page.onShareAppMessage() 事件，如果当前页面没有定义此事件，则点击后无效果 
 
8.Tip

    不自定义转发图片的情况下，默认会取当前页面，从顶部开始，高度为 80% 屏幕宽度的图像作为转发图片。
	转发的调试支持请查看 普通转发的调试支持 和 带 shareTicket 的转发
	只有转发到群聊中打开才可以获取到 shareTickets 返回值，单聊没有 shareTickets
	shareTicket 仅在当前小程序生命周期内有效
	由于策略变动，小程序群相关能力进行调整，开发者可先使用wx.getShareInfo接口中的群ID进行功能开发
六、收获地址和获取二维码
1.wx.chooseAddress - 编辑用户收货地址
	
	调起用户编辑收货地址原生界面，并在编辑完成后返回用户选择的地址；
	需要用户授权 scope.address；
	object参数说明：
		
	success参数说明:
	
	eg：
	
2.获取二维码

3.获取小程序码

4.获取小程序二维码


七、卡券和发票		
1.wx.addCard - 批量添加卡券

2.wx.openCard - 查看微信卡包中的卡券

3.会员卡组件
 
4.wx.chooseInvoiceTitle - 选择用户的发票抬头

八、设置和打开程序		
1.wx.openSetting - 调起客户端小程序设置界面，返回用户设置的操作结果

2.wx.getSetting - 获取用户的当前设置

3.wx.navigateToMiniProgram - 打开同一公众号下关联的另一个小程序

	此接口即将废弃，请使用 <navigator> 组件来使用此功能
4.wx.navigateBackMiniProgram - 返回到上一个小程序，只有在当前小程序是被其他小程序打开时可以调用成功


5.launchApp - 打开APP

 
 
 
 
参考资料：		
1.[开发接口](https://developers.weixin.qq.com/miniprogram/dev/api/api-login.html)		
2.[UnionID机制说明](https://developers.weixin.qq.com/miniprogram/dev/api/unionID.html)		
3.[小程序支付接口文档](https://pay.weixin.qq.com/wiki/doc/api/wxa/wxa_api.php?chapter=7_7&index=3)		
4.[]()		



