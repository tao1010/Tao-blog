---
title: 微信小程序-API-网络
date: 2018-05-18 13:03:03
tags:	微信小程序
categories: 小程序
---

一、网络API简介		
1.服务器域名配置	
	
	每个微信小程序需要事先设置一个通讯域名,小程序可以跟指定的域名进行网络通信;
	包括：
		普通HTTP请求(request)
		上传文件(uploadFile)	
		下载文件(downloadFile)
		WebSocket(connectSocket)
2.配置流程		
	
	配置路径:小程序后台-->设置-->开发设置-->服务器域名；
	注意事项：
		域名只支持https(request、uploadFile、downloadFile)和wss(connectSocket)协议;
		域名不能使用IP地址和localhost，且不能带端口号;
		域名必须经过ICP备案;
		api.weixin.qq.com不能被配置为服务器域名，相关API也不能在小程序内调用;
		开发者应将appsecret保存到后台服务器中m通过服务器使用appsecret获取accesstoken,并调用相关API;
		每个接口，分别可以配置最多20个域名;
3.HTTPS证书
	
	小程序必须使用HTTPS请求，小程序内会对服务器域名使用的HTTPS证书进行校验，失败则请求不能成功发起；
	为了保证小程序兼容性，建议开发者以最高标准进行证书配置，并使用相关工具检查现有证书是否符合要求；
	证书要求:
		HTTPS证书必须有效;
			证书必须被系统信任；
			部署SSL证书的网站域名必须与证书颁发的域名一致；
			证书必须在有效期内；
		iOS不支持自签名证书;
		iOS下证书必须满足ATS-apple transport security 的要求；
		TLS必须支持1.2及以上版本；
		部分CA可能不被操作系统信任；
4.跳过域名校验		
	
	微信开发工具中，可以临时开启: 开发环境不校验请求域名、TLS版本及HTTPS证书 选项，跳过服务器域名校验；
	服务器域名配置成功后，建议关闭此选项，在各平台下测试，确认域名配置正确；
	如果手机上出现 “打开调试模式可以发出请求，关闭调试模式无法发出请求” 的现象，请确认是否跳过了域名校验，并确认服务器域名和证书配置是否正确
5.关于请求		

	默认超时时间和最大超时时间 60s;
	reuqest、uploadFile、downloadFile 的最大并发限制10个;
	网络请求的referer header 不可设置;
		固定格式：https://servicewechat.com/{appid}/{version}/page-frame.html
			{appid} 为小程序的 appid，
			{version} 为小程序的版本号，
			版本号为 0 表示为开发版、体验版以及审核版本，
			版本号为 devtools 表示为开发者工具，其余为正式版本。
	小程序进入后台运行后（非置顶聊天），如果 5s 内网络请求没有结束，会回调错误信息 fail interrupted；在回到前台之前，网络请求接口调用都会无法调用
6.关于服务器返回	

	返回值编码
		建议服务器返回值使用 UTF-8 编码。对于非 UTF-8 编码，小程序会尝试进行转换，但是会有转换失败的可能;
		小程序会自动对 BOM 头进行过滤;
	回调
		只要成功接收到服务器返回，无论statusCode是多少，都会进入success回调;

二、发起请求	
1.wx.request 发起网络请求		
2.参数说明

|参数名|类型|必填|默认值|描述|
|---|---|---|---|---|
|url|String|是||开发者服务器接口地址,url 中不能有端口|
|data|Object/String/ArrayBuffer|否||请求的参数|
|header| Object |否||设置请求的 header，header 中不能设置 Referer,content-type 默认为 'application/json';|
|method| String |否|GET|（需大写）有效值：OPTIONS, GET, HEAD, POST, PUT, DELETE, TRACE, CONNECT|
|dataType| String |否|json|如果设为json，会尝试对返回的数据做一次 JSON.parse|
|responseType| String |否|text|设置响应的数据类型。合法值：text、arraybuffer|
|success| Function |否||调用成功的回调函数|
|fail| Function |否||调用失败的回调函数|
|complete|Function|否||调用结束的回调函数（调用成功、失败都会执行）|

3.success返回参数说明

|参数|类型|说明|
|---|---|---|
|data|Object/String/ArrayBuffer|开发者服务器返回的数据|
|statusCode|Number|开发者服务器返回的 HTTP 状态码|
|header|Object|开发者服务器返回的 HTTP Response Header|
data数据说明:
	
	最终发送给服务器的数据是 String 类型，如果传入的 data 不是 String 类型，会被转换成 String 。
	转换规则如下：
		对于 GET 方法的数据，会将数据转换成 query string（encodeURIComponent(k)=encodeURIComponent(v)&encodeURIComponent(k)=encodeURIComponent(v)...）
		对于 POST 方法且 header['content-type'] 为 application/json 的数据，会对数据进行 JSON 序列化
		对于 POST 方法且 header['content-type'] 为 application/x-www-form-urlencoded 的数据，会将数据转换成 query string （encodeURIComponent(k)=encodeURIComponent(v)&encodeURIComponent(k)=encodeURIComponent(v)...）
4.中断请求任务		

	返回一个requestTask对象，通过requestTask可中断请求任务；
	requestTask对象方法列表：
		abort - 中断请求任务；
5.eg
	
```js
//网络请求
wx.request({
  url: 'test.php', //仅为示例，并非真实的接口地址
  data: {
     x: '' ,
     y: ''
  },
  header: {
      'content-type': 'application/json' // 默认值
  },
  success: function(res) {
    console.log(res.data)
  }
})

//中断网络任务
const requestTask = wx.request({
  url: 'test.php', //仅为示例，并非真实的接口地址
  data: {
     x: '' ,
     y: ''
  },
  header: {
      'content-type': 'application/json'
  },
  success: function(res) {
    console.log(res.data)
  }
})
requestTask.abort() // 取消请求任务
```		

三、上传下载	
1.wx.uploadFile 上传文件
	
	将本地资源上传到开发者服务器，客户端发起一个 HTTPS POST 请求；
	其中 content-type 为 multipart/form-data；
	上传文件返回一个uploadTask对象，该对象方法列表如下：
		onProgressUpdate 监听上传进度变化（callback）
			参数说明：
				progress  上传进度百分比 Number
				totalBytesSent 已上传的数据长度，单位Bytes Number
				totalBytesExpectedTosend 预期需要上传的数据总长度 单位Bytes Number
		abort 中断上传任务
2.wx.downloadFile下载文件

	下载文件资源到本地，客户端直接发起一个 HTTP GET 请求，返回文件的本地临时路径；
	下载文件返回一个downloadTask对象，该对象方法列表如下：
		onProgressUpdate 监听下载进度变化（callback）			参数说明：
				progress  下载进度百分比 Number
				totalBytesSent 已下载的数据长度，单位Bytes Number
				totalBytesExpectedTosend 预期需要下载的数据总长度 单位Bytes Number
		abort 中断下载任务
3.object参数说明

|参数|类型|必填|说明|
|---|---|---|----|
|uploadFile-上传||||
|url|String|是|开发者服务器url|
|filePath| String |是|要上传文件资源的路径|
|name| String |是|文件对应的key，服务器端可以通过这个key获取文件二进制内容|
|header|Object|否|HTTP请求Header，header中不能设置Referer|
|formDataObject|否|HTTP请求中其他额外的form data|
|success| Function |否|调用成功回调函数|
|fail|Function|否|调用失败回调函数|
|complte|Function|否|调用结束回调函数(不管成功、失败、都执行)|
|downloadFile-下载文件||||
url|String|是|下载资源的url|
|header|Object|否|HTTP请求Header，header中不能设置Referer|
|success| Function |否|下载成功后以 tempFilePath 的形式传给页面，res = {tempFilePath: '文件的临时路径'}|
|fail|Function|否|调用失败回调函数|
|complte|Function|否|调用结束回调函数(不管成功、失败、都执行)|

4.success参数说明	

|参数|类型|描述|
|---|---|---|
|uploadFile-上传文件|||
|data|String|服务器返回的数据|
|statusCode|Number|服务器返回的HTTP状态码|
|downloadFile-下载文件|||
| tempFilePath |String|临时文件路径，下载后的文件会存储到一个临时文件|
|statusCode|Number|服务器返回的HTTP状态码|

5.注意

	文件的临时路径，在小程序本次启动期间可以正常使用，如需持久保存，需在主动调用 wx.saveFile，才能在小程序下次启动时访问得到。 
	请在 header 中指定合理的 Content-Type 字段，以保证客户端正确处理文件类型
	6.5.3 以及之前版本的 iOS 微信客户端 header 设置无效
6.eg

```js
上传文件
//上传文件
uploadFile:function(){

    const uploadTask =wx.uploadFile({
      url: '',//上传服务器的URL
      filePath: '',//文件路径
      name: '',//文件名称
      header: {},
      formData: {
        'user': 'test'
      },
      success: function(res) {

        var data = res.data
        //do something
      },
      fail: function(res) {
        //do something
      },
      complete: function(res) {
        //do something
      },
    })
    //上传进度
    uploadTask.onProgressUpdate((res) =>{

      console.log('上传进度', res.progress)
      console.log('已经上传的数据长度', res.totalBytesSent)
      console.log('预期需要上传的数据总长度', res.totalBytesExpectedToSend)
    })
    uploadTask.abort()//取消上传任务
 },

下载文件
downloadFile: function(){
	const downloadTask = wx.downloadFile({
	    url: 'http://example.com/audio/123', //仅为示例，并非真实的资源
	    success: function(res) {
	        wx.playVoice({
	            filePath: res.tempFilePath
	        })
	    }
	})
	
	downloadTask.onProgressUpdate((res) => {
	    console.log('下载进度', res.progress)
	    console.log('已经下载的数据长度', res.totalBytesWritten)
	    console.log('预期需要下载的数据总长度', res.totalBytesExpectedToWrite)
	})
	
	downloadTask.abort() // 取消下载任务
	
	}
```

四、WebSocket
1.wx.connectSocket - 创建WebSocket连接		
	
	返回SocketTask对象，WebSocket任务；
	对象方法列表：
		SocketTask.send(object) - 通过 WebSocket 连接发送数据。
		SocketTask.close(object) - 关闭 WebSocket 连接
		SocketTask.onOpen - 监听 WebSocket 连接打开事件
		SocketTask.onClose - 监听 WebSocket 连接关闭事件
		SocketTask.onError - 监听 WebSocket 错误
		SocketTask.onMessage - 监听WebSocket接受到服务器的消息事件
2.wx.onSocketOpen(CALLBACK) - 监听WebSocket连接打开事件
	
	callback回调函数 res header object 连接成功的HTTP响应Header
3.wx.onSocketError(CALLBACK) - 监听WebSocket错误


4.wx.sendSocketMessage(OBJECT) - 通过 WebSocket 连接发送数据

	需要先wx.connectSocket，并在wx.onSocketOpen回调之后才能发送；
object参数说明：	
	
|参数|类型|必填|描述|
|---|---|---|---|
|data|String/ArrayBuffer|是|需要发送的内容|
|success| Function |否|调用成功回调函数|
|fail| Function |否|调用失败回调函数|
|complete|Function|否|调用结束回调函数(不管成功、失败、都执行)|
5.wx.onSocketMessage(CALLBACK) - 监听WebSocket接受到服务器的消息事件

6.wx.closeSocket(OBJECT) - 关闭 WebSocket 连接。

|参数|类型|必填|描述|
|---|---|---|---|
|code|Number|否|一个数字值表示关闭连接的状态号，表示连接被关闭的原因。如果这个参数没有被指定，默认的取值是1000 （表示正常连接关闭）|
|reason|String|否|一个可读的字符串，表示连接被关闭的原因。这个字符串必须是不长于123字节的UTF-8 文本（不是字符）|
|success| Function |否|调用成功回调函数|
|fail| Function |否|调用失败回调函数|
|complete|Function|否|调用结束回调函数(不管成功、失败、都执行)|

7.wx.onSocketClose(CALLBACK) - 监听WebSocket关闭	
8.注意：
	
	v1.7.0-  小程序同时只能有一个WebSocket连接，如果当前已经存在一个WebSocket连接，会自动关闭该链接，并重新创建一个WebSocket连接;
	v1.7.0+ 支持存在多个WebSocket连接，每次成功调用wx.connectSocket会返回一个新的SocketTask；
	时序问题：如果 wx.connectSocket 还没回调 wx.onSocketOpen，而先调用 wx.closeSocket，那么就做不到关闭 WebSocket 的目的。
	
9.eg

```objectivec
//连接
wx.connectSocket({
  url: 'wss://example.qq.com',
  data:{
    x: '',
    y: ''
  },
  header:{ 
    'content-type': 'application/json'
  },
  protocols: ['protocol1'],
  method:"GET"
})

//定义变量
var socketOpen = false
var socketMsgQueue = []

//监听WebSocket连接打开事件
wx.onSocketOpen(function(res) {
  console.log('WebSocket连接已打开！')
  
  //发送消息
  socketOpen = true
  for (var i = 0; i < socketMsgQueue.length; i++){
  	
  	 if (socketOpen) {
	    wx.sendSocketMessage({
	      data:socketMsgQueue[i]
	    })
	  } else {
	     socketMsgQueue.push(socketMsgQueue[i])
	  }
  }
  socketMsgQueue = []
  //必须在 WebSocket 打开期间调用 wx.closeSocket 才能关闭。
  wx.closeSocket()
})

//监听WebSocket接受到服务器的消息事件
wx.onSocketMessage(function(res) {
  console.log('收到服务器内容：' + res.data)
})

//监听WebSocket错误
wx.onSocketError(function(res){
  console.log('WebSocket连接打开失败，请检查！')
})

//关闭Socket
wx.onSocketClose(function(res) {
  console.log('WebSocket 已关闭！')
})

```

参考资料：		
1.[网络API](https://developers.weixin.qq.com/miniprogram/dev/api/api-network.html)		