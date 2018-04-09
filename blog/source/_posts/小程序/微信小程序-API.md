---
title: 微信小程序-API
date: 2018-04-09 13:18:04
tags: 微信小程序
categories: 小程序
---

说明：
	
	wx.on 开头的 API 是监听某个事件发生的API接口，接受一个 CALLBACK 函数作为参数。当该事件触发时，会调用 CALLBACK 函数。
	如未特殊约定，其他 API 接口都接受一个OBJECT作为参数。
	OBJECT中可以指定success, fail, complete（成功、失败都会执行）来接收接口调用结果。

一、网络

|API|说明|
|---|---|
|[wx.request](https://developers.weixin.qq.com/miniprogram/dev/api/network-request.html)|发起网络请求|
|[wx.uploadFile](https://developers.weixin.qq.com/miniprogram/dev/api/network-file.html#wxuploadfileobject)|上传文件|
|[wx.downloadFile](https://developers.weixin.qq.com/miniprogram/dev/api/network-file.html#wxdownloadfileobject)|下载文件|
|[wx.connectSocket](https://developers.weixin.qq.com/miniprogram/dev/api/network-socket.html#wxconnectsocketobject)|创建WebSocket连接|
|[wx.onSocketOpen](https://developers.weixin.qq.com/miniprogram/dev/api/network-socket.html#wxonsocketopencallback)|监听WebSocket打开|
|[wx.onSocketError](https://developers.weixin.qq.com/miniprogram/dev/api/network-socket.html#wxonsocketerrorcallback)|监听WebSocket错误|
|[wx.sendSocketMessage](https://developers.weixin.qq.com/miniprogram/dev/api/network-socket.html#wxsendsocketmessageobject)|发送WebSocket消息|
|[wx.onSocketMessage](https://developers.weixin.qq.com/miniprogram/dev/api/network-socket.html#wxonsocketmessagecallback)|接受WebSocket消息|
|[wx.closeSocket](https://developers.weixin.qq.com/miniprogram/dev/api/network-socket.html#wxclosesocket)|关闭WebSocket连接|
|[wx.onSocketClose](https://developers.weixin.qq.com/miniprogram/dev/api/network-socket.html#wxonsocketclosecallback)|监听WebSocket关闭|

二、媒体

|API|说明|
|---|---|
|[wx.chooseImage](https://developers.weixin.qq.com/miniprogram/dev/api/media-picture.html#wxchooseimageobject)|从相册选择图片或者拍照|
|[wx.priviewImage](https://developers.weixin.qq.com/miniprogram/dev/api/media-picture.html#wxpreviewimageobject)|预览图片|
|[wx.startRecord](https://developers.weixin.qq.com/miniprogram/dev/api/media-record.html#wxstartrecordobject)|开始录音|
|[wx.stopRecord](https://developers.weixin.qq.com/miniprogram/dev/api/media-record.html#wxstoprecord)|结束录音|
|[wx.playVoice](https://developers.weixin.qq.com/miniprogram/dev/api/media-voice.html#wxplayvoice)|播放语音|
|[wx.pauseVoice](https://developers.weixin.qq.com/miniprogram/dev/api/media-voice.html#wxpausevoice)|暂停播放语音|
|[wx.stopVoice](https://developers.weixin.qq.com/miniprogram/dev/api/media-voice.html#wxstopvoice)|结束播放语音|
|[wx.getBackgroudAudioPlayerState](https://developers.weixin.qq.com/miniprogram/dev/api/media-background-audio.html#wxgetbackgroundaudioplayerstateobject)|获取音乐播放状态|
|[wx.playBackgroundAudio](https://developers.weixin.qq.com/miniprogram/dev/api/media-background-audio.html#wxplaybackgroundaudioobject)|播放音乐|
|[wx.pauseBackgroundAudio](https://developers.weixin.qq.com/miniprogram/dev/api/media-background-audio.html#wxpausebackgroundaudio)|暂停播放音乐|
|[wx.seekBackgroundAudio](https://developers.weixin.qq.com/miniprogram/dev/api/media-background-audio.html#wxstopbackgroundaudio)|控制音乐播放进度|
|[wx.stopBackgroundAudio](https://developers.weixin.qq.com/miniprogram/dev/api/media-background-audio.html#wxstopbackgroundaudio)|停止播放音乐|
|[wx.onBackgroundAudioPlay](https://developers.weixin.qq.com/miniprogram/dev/api/media-background-audio.html#wxonbackgroundaudioplaycallback)|监听音乐开始播放|
|[wx.onBackgroundAudioPause](https://developers.weixin.qq.com/miniprogram/dev/api/media-background-audio.html#wxonbackgroundaudiopausecallback)|监听音乐暂停|
|[wx.onBackgroundAudioStop](https://developers.weixin.qq.com/miniprogram/dev/api/media-background-audio.html#wxonbackgroundaudiostopcallback)|监听音乐结束|
|[wx.chooseVideo](https://developers.weixin.qq.com/miniprogram/dev/api/media-video.html)|从相册选择视频或拍摄|

三、文件

|API|说明|
|---|---|
|[wx.saveFile](https://developers.weixin.qq.com/miniprogram/dev/api/file.html)|保存文件|
|[wx.getSavedFileList](https://developers.weixin.qq.com/miniprogram/dev/api/file.html#wxgetsavedfilelistobject)|获取已保存文件列表|
|[wx.getSavedFileInfo](https://developers.weixin.qq.com/miniprogram/dev/api/file.html#wxgetsavedfileinfoobject)|获取已保存文件信息|
|[wx.removeSaveFile](https://developers.weixin.qq.com/miniprogram/dev/api/file.html#wxremovesavedfileobject)|删除已保存文件|
|[wx.openDocument](https://developers.weixin.qq.com/miniprogram/dev/api/file.html#wxopendocumentobject)|打开文件|

四、数据缓存

|API| 异步或同步|说明|
|---|---|---|
|[wx.getStorage](https://developers.weixin.qq.com/miniprogram/dev/api/data.html#wxgetstorageobject)|异步|获取本地数据缓存|
|[wx.getStorageSync](https://developers.weixin.qq.com/miniprogram/dev/api/data.html#wxgetstoragesynckey)|异步|获取本地数据缓存|
|[wx.setStorage](https://developers.weixin.qq.com/miniprogram/dev/api/data.html#wxsetstorageobject)|异步|设置本地数据缓存|
|[wx.setStorageSync](https://developers.weixin.qq.com/miniprogram/dev/api/data.html#wxsetstoragesynckeydata)|同步|设置本地数据缓存|
|[wx.getStorageInfo](https://developers.weixin.qq.com/miniprogram/dev/api/data.html#wxgetstorageinfoobject)|异步|获取本地缓存信息|
|[wx.getStorageInfoSync](https://developers.weixin.qq.com/miniprogram/dev/api/data.html#wxgetstorageinfosync)|同步|获取本地缓存信息|
|[wx.removeStorage](https://developers.weixin.qq.com/miniprogram/dev/api/data.html#wxremovestorageobject)|异步|删除本地缓存内容|
|[wx.removeStorageSync](https://developers.weixin.qq.com/miniprogram/dev/api/data.html#wxremovestoragesynckey)|同步|删除本地缓存内容|
|[wx.clearStorage](https://developers.weixin.qq.com/miniprogram/dev/api/data.html#wxclearstorageobject)|异步|清理本地数据缓存|
|[wx.clearStorageSync](https://developers.weixin.qq.com/miniprogram/dev/api/data.html#wxclearstoragesync)|同步|清理本地数据缓存|

五、位置

|API|说明|
|---|---|
|[wx.getLocation](https://developers.weixin.qq.com/miniprogram/dev/api/location.html#wxgetlocationobject)|获取当前位置|
|[wx.chooseLocation](https://developers.weixin.qq.com/miniprogram/dev/api/location.html#wxchooselocationobject)|打开地图选择位置|
|[wx.openLocation](https://developers.weixin.qq.com/miniprogram/dev/api/location.html#wxopenlocationobject)|打开内置地图|
|[wx.createMapContext](https://developers.weixin.qq.com/miniprogram/dev/api/api-map.html#wxcreatemapcontextmapid)|地图组件控制|

六、设备

|API|说明|
|---|---|
|[wx.getNetworkType](https://developers.weixin.qq.com/miniprogram/dev/api/device.html#wxgetnetworktypeobject)|获取网络类型|
|[wx.onNetworkStatusChange](https://developers.weixin.qq.com/miniprogram/dev/api/device.html#wxonnetworkstatuschangecallback)|监听网络状态变化|
|[wx.getSystemInfo](https://developers.weixin.qq.com/miniprogram/dev/api/systeminfo.html#wxgetsysteminfoobject)|获取系统信息 - 异步|
|[wx.getSystemInfoSync](https://developers.weixin.qq.com/miniprogram/dev/api/systeminfo.html#wxgetsysteminfosync)|获取系统信息 - 同步|
|[wx.onAccelerometerChange](https://developers.weixin.qq.com/miniprogram/dev/api/accelerometer.html#wxonaccelerometerchangecallback)|监听加速度数据|
|[wx.startAccelerometer](https://developers.weixin.qq.com/miniprogram/dev/api/accelerometer.html#wxstartaccelerometerobject)|开始加速度数据|
|[wx.stopAccelerometer](https://developers.weixin.qq.com/miniprogram/dev/api/accelerometer.html#wxstopaccelerometerobject)|停止加速度数据|
|[wx.onCompassChange](https://developers.weixin.qq.com/miniprogram/dev/api/compass.html#wxoncompasschangecallback)|监听罗盘数据|
|[wx.startCompass](https://developers.weixin.qq.com/miniprogram/dev/api/compass.html#wxstartcompassobject)|开启罗盘|
|[wx.stopCompass](https://developers.weixin.qq.com/miniprogram/dev/api/compass.html#wxstopcompassobject)|停止罗盘|
|[wx.setClipboardData](https://developers.weixin.qq.com/miniprogram/dev/api/clipboard.html#wxsetclipboarddataobject)|设置剪贴板内容|
|[wx.getClipboardData](https://developers.weixin.qq.com/miniprogram/dev/api/clipboard.html#wxsetclipboarddataobject)|获取剪贴板内容|
|[wx.makePhoneCall](https://developers.weixin.qq.com/miniprogram/dev/api/phonecall.html#wxmakephonecallobject)|拨打电话|
|[wx.scanCode](https://developers.weixin.qq.com/miniprogram/dev/api/scancode.html#wxscancodeobject)|扫码|

七、界面

|API|说明|
|---|---|
|[wx.showToast](https://developers.weixin.qq.com/miniprogram/dev/api/api-react.html#wxshowtoastobject)|显示提示框|
|[wx.showLoading](https://developers.weixin.qq.com/miniprogram/dev/api/api-react.html#wxshowloadingobject)|显示加载提示框|
|[wx.hideToast](https://developers.weixin.qq.com/miniprogram/dev/api/api-react.html#wxhidetoast)|隐藏提示框|
|[wx.hideLoading](https://developers.weixin.qq.com/miniprogram/dev/api/api-react.html#wxhideloading)|隐藏加载提示框|
|[wx.showModal](https://developers.weixin.qq.com/miniprogram/dev/api/api-react.html#wxshowmodalobject)|显示模态弹窗|
|[wx.showActionSheet](https://developers.weixin.qq.com/miniprogram/dev/api/api-react.html#wxshowactionsheetobject)|显示菜单列表|
|[wx.setNavigationBarTitle](https://developers.weixin.qq.com/miniprogram/dev/api/ui.html#wxsetnavigationbartitleobject)|设置当前页面标题|
|[wx.showNavigationBarLoading](https://developers.weixin.qq.com/miniprogram/dev/api/ui.html#wxshownavigationbarloading)|显示导航条加载动画|
|[wx.hideNavigationBarLoading](https://developers.weixin.qq.com/miniprogram/dev/api/ui.html#wxhidenavigationbarloading)|隐藏导航条加载动画|
|[wx.navigateTo](https://developers.weixin.qq.com/miniprogram/dev/api/ui-navigate.html#wxnavigatetoobject)|新窗口打开页面|
|[wx.redirectTo](https://developers.weixin.qq.com/miniprogram/dev/api/ui-navigate.html#wxredirecttoobject)|原窗口打开页面|
|[wx.switchTab](https://developers.weixin.qq.com/miniprogram/dev/api/ui-navigate.html#wxswitchtabobject)|切换到tabbar页面|
|[wx.navigateBack](https://developers.weixin.qq.com/miniprogram/dev/api/ui-navigate.html#wxnavigateback)|退回上一个页面|
|[wx.createAnimation](https://developers.weixin.qq.com/miniprogram/dev/api/api-animation.html)|动画|
|[wx.createContext](https://developers.weixin.qq.com/miniprogram/dev/api/canvas/create-canvas-context.html)|创建绘图上下文|
|[wx.drawCanvas](https://developers.weixin.qq.com/miniprogram/dev/api/canvas/draw-canvas.html)|绘图|
|[wx.stopPullDownRefresh](https://developers.weixin.qq.com/miniprogram/dev/api/pulldown.html#wxstoppulldownrefresh)|停止下拉刷新动画|

八、第三方平台

|API|说明|
|---|---|
|[wx.getExtConfig]()|获取[第三方平台](https://developers.weixin.qq.com/miniprogram/dev/devtools/ext.html)自定义的数据字段|
|[wx.getExtConfigSync]()||

九、开放接口

|API|说明|
|---|---|
|[wx.login](https://developers.weixin.qq.com/miniprogram/dev/api/api-login.html)|登录|
|[wx.getUserInfo](https://developers.weixin.qq.com/miniprogram/dev/api/open.html#wxgetuserinfoobject)|获取用户信息|
|[wx.chooseAddress](https://developers.weixin.qq.com/miniprogram/dev/api/address.html#wxchooseaddressobject)|获取用户收货地址|
|[wx.requestPayment](https://developers.weixin.qq.com/miniprogram/dev/api/api-pay.html#wxrequestpaymentobject)|发起微信支付|
|[wx.addCard](https://developers.weixin.qq.com/miniprogram/dev/api/card.html#wxaddcardobject)|添加卡券|
|[wx.openCard](https://developers.weixin.qq.com/miniprogram/dev/api/card.html#wxopencardobject)|打开卡券|

十、数据

|API|说明|
|---|---|
|[wx.reportAnalytics](https://developers.weixin.qq.com/miniprogram/dev/api/analysis-report.html)|自定义分析数据上报接口|

十一、更新

|API|说明|
|---|---|
|[wx.getUpdateManager](https://developers.weixin.qq.com/miniprogram/dev/api/getUpdateManager.html)|获取全局唯一的版本更新管理器|

十二、多线程

|API|说明|
|---|---|
|[wx.createWorker](https://developers.weixin.qq.com/miniprogram/dev/api/createWorker.html)|多线程worker|


十三、调试接口

|API|说明|
|---|---|
|[wx.setEnableDebug](https://developers.weixin.qq.com/miniprogram/dev/api/setEnableDebug.html)|设置是否打开调试开关，此开关对正式版也能生效|

参考资料：   
1.[微信小程序API文档](https://developers.weixin.qq.com/miniprogram/dev/api/)

