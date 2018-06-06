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
通过后台接口可以获取小程序任意页面的二维码，扫描该二维码可以直接进入小程序对应的页面;

	A接口，生成小程序码，可接受path参数较长，生成个数受限。 
	B接口，生成小程序码，可接受页面参数较短，生成个数不受限。 
	C接口，生成二维码，可接受path参数较长，生成个数受限；
	需要配置域名 https://api.weixin.qq.com；
	接口A加上接口C，总共生成的码数量限制为100,000；
	通过该接口，仅能生成已发布的小程序的二维码。可以在开发者工具预览时生成开发版的带参二维码。
	POST 参数需要转成 json 字符串，不支持 form 表单提交。
	auto_color line_color 参数仅对小程序码生效；
3.获取小程序码
	
	接口A: 
		适用于需要的码数量较少的业务场景 
		通过该接口生成的小程序码，永久有效，数量限制见文末说明，请谨慎使用。用户扫描该码进入小程序后，将直接进入 path 对应的页面;
	接口地址：
		https://api.weixin.qq.com/wxa/getwxacode?access_token=ACCESS_TOKEN
	POST参数说明:
		path	不能为空，最大长度 128 字节 String
		width	二维码的宽度 默认430 int
		auto_color	自动配置线条颜色，如果颜色依然是黑色，则说明不建议配置主色调 默认false Bool
		line_color   auth_color 为 false 时生效，使用 rgb 设置颜色 例如 {"r":"xxx","g":"xxx","b":"xxx"},十进制表示 默认{'r':'0','g':'0','b':'0'}. Object
		is_hyaline	是否需要透明底色， is_hyaline 为true时，生成透明底色的小程序码 默认false Bool
		
	接口B：
		适用于需要的码数量极多的业务场景
		通过该接口生成的小程序码，永久有效，数量暂无限制。
		用户扫描该码进入小程序后，开发者需在对应页面获取的码中 scene 字段的值，再做处理逻辑。使用如下代码可以获取到二维码中的 scene 字段的值。
		调试阶段可以使用开发工具的条件编译自定义参数 scene=xxxx 进行模拟，开发工具模拟时的 scene 的参数值需要进行 urlencode
	接口地址：
		https://api.weixin.qq.com/wxa/getwxacodeunlimit?access_token=ACCESS_TOKEN
	POST 参数说明
		scene	String		最大32个可见字符，只支持数字，大小写英文以及部分特殊字符：!#$&'()*+,/:;=?@-._~，其它字符请自行编码为合法字符（因不支持%，中文无法使用 urlencode 处理，请使用其他编码方式）
		page	String		必须是已经发布的小程序存在的页面（否则报错），例如 "pages/index/index" ,根路径前不要填加'/',不能携带参数（参数请放在scene字段里），如果不填写这个字段，默认跳主页面
		width	Int	430	二维码的宽度
		auto_color	Bool	false	自动配置线条颜色，如果颜色依然是黑色，则说明不建议配置主色调
		line_color	Object	{"r":"0","g":"0","b":"0"}		auto_color 为 false 时生效，使用 rgb 设置颜色 例如 {"r":"xxx","g":"xxx","b":"xxx"} 十进制表示
		is_hyaline	Bool	false	是否需要透明底色， is_hyaline 为true时，生成透明底色的小程序码
	eg:
		// 这是首页的 js
		Page({
		  onLoad: function(options) {
		    // options 中的 scene 需要使用 decodeURIComponent 才能获取到生成二维码时传入的 scene
		    var scene = decodeURIComponent(options.scene)
		  }
		})
		
4.获取小程序二维码
	
	接口C：
		适用于需要的码数量较少的业务场景
		通过该接口生成的小程序二维码，永久有效。用户扫描该码进入小程序后，将直接进入 path 对应的页面
	接口地址：
		https://api.weixin.qq.com/cgi-bin/wxaapp/createwxaqrcode?access_token=ACCESS_TOKEN
	POST参数说明:
		path	不能为空，最大长度 128 字节 String
		width	二维码的宽度 默认430 int
	eg：
		{"path": "pages/index?query=1", "width": 430}
		//pages/index 需要在 app.json 的 pages 中定义
5.错误码
	
	45009：B接口调用分钟频率受限(目前5000次/分钟，会调整)，如需大量小程序码，建议预生成。
	45029：A接口和C接口生成码个数总和到达最大个数限制。 
	41030：B接口所传page页面不存在，或者小程序没有发布，请注意B接口没有path参数，传path参数虽然可以生成小程序码，但是只能跳主页面
	
七、卡券和发票		
只有认证小程序才能使用卡券接口			
1.wx.addCard - 批量添加卡券
	
	object参数说明:
		cardList 需要添加的卡券列表 必填 ObjectArray
			cardId  卡券Id String
			cardExt 卡券的扩展参数 String
				code	用户领取的 code，仅自定义 code 模式的卡券须填写，非自定义 code 模式卡券不可填写
				openid 指定领取者的openid，只有该用户能领取。 bind_openid 字段为 true 的卡券必须填写，bind_openid 字段为 false 不可填写
				timestamp 时间戳，东八区时间,UTC+8，单位为秒 必填
				noce_str 随机字符串，由开发者设置传入，加强安全性（若不填写可能被重放请求）。随机字符串，不长于 32 位。推荐使用大小写字母和数字，不同添加请求的 nonce_str 须动态生成，若重复将会导致领取失败
				fixed_begintimestamp 卡券在第三方系统的实际领取时间，为东八区时间戳（UTC+8,精确到秒）。当卡券的有效期类为 DATE_TYPE_FIX_TERM 时专用，标识卡券的实际生效时间，用于解决商户系统内起始时间和领取微信卡券时间不同步的问题
				outer_str	领取渠道参数，用于标识本次领取的渠道值
				signature	签名，商户将接口列表中的参数按照指定方式进行签名,签名方式使用 SHA1 必填
		sucess/fail/complete 回调函数
	success返回参数：
		cardList 卡券添加结果列表
			code		加密 code，为用户领取到卡券的code加密后的字符串
			cardId		用户领取到卡券的Id
			cardExt	用户领取到卡券的扩展参数，与调用时传入的参数相同
			isSuccess 是否成功
	eg：
	wx.addCard({
      cardList: [
        {
          carId: '',
          cardExt: '{"code": "", "openid": "", "timestamp": "", "signature":""}'
        }
      ],
      success: function(res){

        console.log('success' + res)
      },
      fail: function(res){
        console.log('fail' + res)
      }
    })
	
2.wx.openCard - 查看微信卡包中的卡券

	object参数说明:
		cardList 需要添加的卡券列表 必填 ObjectArray
			cardId		需要打开的卡券 Id
			code		由 addCard 的返回对象中的加密 code 通过解密后得到
		sucess/fail/complete 回调函数
	eg：
	 wx.openCard({
      cardList: [{cardId:'',code:''}],
      success: function (res) {
        console.log('success' + res)
      },
      fail: function (res) {
        console.log('fail' + res)
      }
    })

3.会员卡组件

 	开发者可以在小程序内调用该接口拉起会员开卡组件，方便用户快速填写会员注册信息并领卡。 
 	该接口拉起开卡组件无须提前将开卡组件和发起小程序绑定至同一个公众号，开发者直接调用即可。
	调用前开发者须完成以下步骤：
		创建一张微信会员卡并设置为一键激活模式；
		设置开卡字段；
		获取开卡组件参数；
	参数说明:
		appId	固定为此appid 必填
		extraData	开卡组件参数，由第3步获取，包含以下三个参数 必填
		encrypt_card_id 加密 card_id，传入前须 urldecode必填
		outer_str	会员卡领取渠道值，会在卡券领取事件回传给商户 必填
		biz		商户公众号标识参数，传入前须 urldecode 必填
		success/fail/complete 回调函数
	返回说明:
		在 App.onShow 里判断从会员开卡小程序返回的数据data
			判断 data.referrerInfo.appId 是否为开卡小程序 appId wxeb490c6f9b154ef9，如果不是则中止判断
			判断是否有 data.referrerInfo.extraData 是否有数据，如果没有，表示用户未激活直接左滑/点返回键返回，或者用户已经激活
			若用户激活成功，可以从 data.referrerInfo.extraData 中获取 activate_ticket card_id code 参数用于下一步操作
	eg：
	 wx.navigateToMiniProgram({
      appId: 'wx933aa61d2fda8d42',
      extraData: data, // 包括 encrypt_card_id, outer_str, biz三个字段，须从 step3 中获得的链接中获取参数
      success: function (res) {
        console.log('success' + res)
      },
      fail: function (res) {
        console.log('fail' + res)
      }
    })
4.wx.chooseInvoiceTitle - 选择用户的发票抬头
	
	需要用户授权scope.invoiceTitle
	object参数说明:
		success/fail/complete 回调函数
	success参数说明：
		type	抬头类型（0：单位，1：个人）
		title	抬头名称
		taxNumber	抬头税号
		companyAddress	单位地址
		telephone	手机号码
		bankName
		bankAccount
		errMsg
	eg：
	wx.getSetting({
      success: function (res) {

        if (res.authSetting['scope.invoiceTitle']) {
          console.log(res)
          wx.chooseInvoiceTitle({
            success: function (res) {
              console.log(res)
            }
          })
        } else {
          wx.authorize({
            scope: 'scope.invoiceTitle',
            success() {
              wx.run()
            }
          })
          console.log('未授权')
        }
      }
    })
八、设置和打开程序		
1.wx.openSetting - 调起客户端小程序设置界面，返回用户设置的操作结果

	此接口即将废弃，请使用 <button> 组件来使用此功能
	设置界面只会出现小程序已经向用户请求过的权限
	object参数说明:
		success/fail/complete 回调函数
	success参数说明:
		authSetting 用户授权结果,其中 key 为 scope 值，value 为 Bool 值，表示用户是否允许授权
	eg：
	wx.openSetting({
      success: function(res){
        console.log(res)
      }
    })
2.wx.getSetting - 获取用户的当前设置
	
	返回值中只会出现小程序已经向用户请求过的权限；
	object参数说明:
		success/fail/complete 回调函数
	success参数说明:
		authSetting 用户授权结果,其中 key 为 scope 值，value 为 Bool 值，表示用户是否允许授权
	eg：
	wx.getSetting({
	  success: (res) => {
	    /*
	     * res.authSetting = {
	     *   "scope.userInfo": true,
	     *   "scope.userLocation": true
	     * }
	     */
	  }
	})
3.wx.navigateToMiniProgram - 打开同一公众号下关联的另一个小程序

	此接口即将废弃，请使用 <navigator> 组件来使用此功能;
	必须是同一公众号下，而非同个 open 账号下;
	在开发者工具上调用此 API 并不会真实的跳转到另外的小程序，但是开发者工具会校验本次调用跳转是否成功;
	开发者工具上支持被跳转的小程序处理接收参数的调试;
	只有同一公众号下的关联的小程序之间才可相互跳转;
	object参数说明:
		appId	要打开的小程序 appId 必填
		path	打开的页面路径，如果为空则打开首页
		extraData	需要传递给目标小程序的数据，目标小程序可在 App.onLaunch()，App.onShow() 中获取到这份数据
		envVersion	要打开的小程序版本，有效值 develop（开发版），trial（体验版），release（正式版） ，仅在当前小程序为开发版或体验版时此参数有效；如果当前小程序是正式版，则打开的小程序必定是正式版。默认值 release
		success/fail/complete 回调函数
	eg:
	wx.navigateToMiniProgram({
	  appId: '',
	  path: 'pages/index/index?id=123',
	  extraData: {
	    foo: 'bar'
	  },
	  envVersion: 'develop',
	  success(res) {
	    // 打开成功
	  }
	})
4.wx.navigateBackMiniProgram - 返回到上一个小程序，只有在当前小程序是被其他小程序打开时可以调用成功

	object参数说明:
		extraData 需要返回给上一个小程序的数据，上一个小程序可在 App.onShow() 中获取到这份数据	
		success/fail/complete 回调函数
	eg:
	wx.navigateBackMiniProgram({
	  extraData: {
	    foo: 'bar'
	  },
	  success(res) {
	    // 返回成功
	  }
	}
5.launchApp - 打开APP		
![launch-app](launch-app.png)		

 	在一个小程序的生命周期内，只有在特定条件下，才具有打开 APP 的能力。 
 	打开 APP 的能力 可以理解为由小程序框架在内部管理的一个状态，为 true 则可以打开 APP，为 false 则不可以打开 APP
 	
 	使用方法：
 	需要将 <button> 组件 open-type 的值设置为 launchApp。如果需要在打开 APP 时向 APP 传递参数，可以设置 app-parameter 为要传递的参数。通过 binderror 可以监听打开 APP 的错误事件

九、附近		

	附近的小程序API，提供给需快速批量创建附近地点的小程序开发者使用，使用前请先调高附近地点额度;
	调高附近地点额度申请方式如下：
	下载《调高地点额度申请表》，填写后发送至 placeofminiprogram@qq.com。
	邮件主题为：“帐号名称”申请调高附近地点额度。审核通过后可调高额度.
1.添加地点	
	
	接口地址：
		https://api.weixin.qq.com/wxa/addnearbypoi?access_token=ACCESS_TOKEN		
2.查看地点列表	
	
	接口地址:
		GET https://api.weixin.qq.com/wxa/getnearbypoilist?page=1&page_rows=20&access_token=ACCESS_TOKEN		
3.删除地点	
	
	接口地址:
		https://api.weixin.qq.com/wxa/delnearbypoi?access_token=ACCESS_TOKEN	
4.展示/取消展示附近的小程序		

	接口地址:
		POST https://api.weixin.qq.com/wxa/setnearbypoishowstatus?access_token=ACCESS_TOKEN
十、内容安全和生物识别	
1.imgSecCheck - 校验一张图片是否含有违法违规内容
	
	图片智能鉴黄；
	敏感人脸识别；
	请求地址:
		GET https://api.weixin.qq.com/wxa/img_sec_check?access_token=ACCESS_TOKEN
	请求参数：
		access_token	接口调用凭证 必填
		media	要检测的图片文件，格式支持PNG、JPEG、JPG、GIF，图片尺寸不超过 750px * 1334px 必填
2.msgSecCheck - 检查一段文本是否含有违法违规内容
	
	用户个人资料违规文字检测；
	媒体新闻类用户发表文章，评论内容检测；
	游戏类用户编辑上传的素材(如答题类小游戏用户上传的问题及答案)检测等
	请求地址:
		GET https://api.weixin.qq.com/wxa/msg_sec_check?access_token=ACCESS_TOKEN
	请求参数：
		access_token	接口调用凭证 必填
		content	要检测的文本内容，长度不超过 500KB 必填
 3.wx.checkIsSupportSoterAuthentication - 获取本机支持的 SOTER 生物认证方式
 	
 	object参数说明:
 		success/fail/complete 回调函数
 	success参数说明:
 		supportMode 该设备支持的可被SOTER识别的生物识别方式
 			fingerPrint	指纹识别
 			facial			人脸识别（暂未支持）
 			speech			声纹识别（暂未支持）
 		errMsg
 	eg：
 	wx.checkIsSupportSoterAuthentication({
	    success(res) {
	        // res.supportMode = [] 不具备任何被SOTER支持的生物识别方式
	        // res.supportMode = ['fingerPrint'] 只支持指纹识别
	        // res.supportMode = ['fingerPrint', 'facial'] 支持指纹识别和人脸识别
	    }
	})
4.wx.startSoterAuthentication - 开始 SOTER 生物认证

	object参数说明:
		requestAuthModes	请求使用的可接受的生物认证方式
			fingerPrint	指纹识别
 			facial			人脸识别（暂未支持）
 			speech			声纹识别（暂未支持）
		challenge	挑战因子。挑战因子为调用者为此次生物鉴权准备的用于签名的字符串关键识别信息，将作为result_json的一部分，供调用者识别本次请求。例如：如果场景为请求用户对某订单进行授权确认，则可以将订单号填入此参数
		authContent 验证描述，即识别过程中显示在界面上的对话框提示内容
 		success/fail/complete 回调函数
 	success参数说明:
 		errCode 
 		authMode
 		resultJSON
 		resultJSONSignature
 		errMsg
 	eg：
 	wx.startSoterAuthentication({
	  requestAuthModes: ['fingerPrint'],
	  challenge: '123456',
	  authContent: '请用指纹解锁',
	  success(res) {
	  }
	})
5.wx.checkIsSoterEnrolledInDevice - 获取设备内是否录入如指纹等生物信息的接口

	object参数说明:
		checkAuthMode 认证方式 必填
			fingerPrint	指纹识别
 			facial			人脸识别（暂未支持）
 			speech			声纹识别（暂未支持）
 		success/fail/complete 回调函数
 	success参数说明:
 		isErrored 是否已录入信息
 		errMsg
 	eg：
 	wx.checkIsSoterEnrolledInDevice({
	    checkAuthMode: 'fingerPrint',
	    success(res) {
	        console.log(res.isEnrolled)
	    }
	})
十一、模版消息和客服消息	
1.[模版信息](https://developers.weixin.qq.com/miniprogram/dev/api/notice.html)
	
	模板推送位置：服务通知
	模板下发条件：用户本人在微信体系内与页面有交互行为后触发，详见下发条件说明
	模板跳转能力：点击查看详情仅能跳转下发模板的该帐号的各个页面
	
	使用说明:
		1.获取模版ID；
			通过模版消息管理接口获取模版ID
			在微信公众平台手动配置获取模版ID
		2.页面的 <form/> 组件，属性report-submit为true时，可以声明为需发模板消息，此时点击按钮提交表单可以获取formId，用于发送模板消息。或者当用户完成支付行为，可以获取prepay_id用于发送模板消息；
		3.调用接口下发模板消息
		
	模版消息管理
		1.获取小程序模版库标题列表
			POST https://api.weixin.qq.com/cgi-bin/wxopen/template/library/list?access_token=ACCESS_TOKEN
		2.获取模板库某个模板标题下关键词库
			POST https://api.weixin.qq.com/cgi-bin/wxopen/template/library/get?access_token=ACCESS_TOKEN
		3.组合模板并添加至帐号下的个人模板库
			POST https://api.weixin.qq.com/cgi-bin/wxopen/template/add?access_token=ACCESS_TOKEN
		4.获取帐号下已存在的模板列表
			POST https://api.weixin.qq.com/cgi-bin/wxopen/template/list?access_token=ACCESS_TOKEN
		5.删除帐号下的某个模板
			POST https://api.weixin.qq.com/cgi-bin/wxopen/template/del?access_token=ACCESS_TOKEN
			
	发送模版消息
		1.获取access_token
			GET https://api.weixin.qq.com/cgi-bin/token?grant_type=client_credential&appid=APPID&secret=APPSECRET
		2.发送模板消息
			POST https://api.weixin.qq.com/cgi-bin/message/wxopen/template/send?access_token=ACCESS_TOKEN
			
	
2.[客服消息](https://developers.weixin.qq.com/miniprogram/dev/api/custommsg/receive.html)

	接收消息和事件
	
	发送客户消息
	
	转发消息
	
	临时素材接口
	
	客服输入状态
	
	接入指引

参考资料：		
1.[开发接口](https://developers.weixin.qq.com/miniprogram/dev/api/api-login.html)		
2.[UnionID机制说明](https://developers.weixin.qq.com/miniprogram/dev/api/unionID.html)		
3.[小程序支付接口文档](https://pay.weixin.qq.com/wiki/doc/api/wxa/wxa_api.php?chapter=7_7&index=3)		
4.[微信卡券接口文档](https://mp.weixin.qq.com/cgi-bin/announce?action=getannouncement&key=1490190158&version=1&lang=zh_CN&platform=2)		
5.[申请微信认证入口](https://developers.weixin.qq.com/miniprogram/product/renzheng.html#一、申请微信认证入口)		



