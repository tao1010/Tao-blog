---
title: 微信小程序-API-
date: 2018-05-04 13:18:04
tags: 微信小程序
categories: 小程序
---

说明：
	
	wx.on 开头的 API 是监听某个事件发生的API接口，接受一个 CALLBACK 函数作为参数。当该事件触发时，会调用 CALLBACK 函数。
	如未特殊约定，其他 API 接口都接受一个OBJECT作为参数。
	OBJECT中可以指定success, fail, complete（成功、失败都会执行）来接收接口调用结果。

一、网络

|API|说明|兼容|
|---|---|---|
|发起请求|
|[wx.request](https://developers.weixin.qq.com/miniprogram/dev/api/network-request.html)|发起网络请求|
|上传、下载|
|[wx.uploadFile](https://developers.weixin.qq.com/miniprogram/dev/api/network-file.html#wxuploadfileobject)|上传文件|
|[wx.downloadFile](https://developers.weixin.qq.com/miniprogram/dev/api/network-file.html#wxdownloadfileobject)|下载文件|
|WebSocket|
|[wx.connectSocket](https://developers.weixin.qq.com/miniprogram/dev/api/network-socket.html#wxconnectsocketobject)|创建WebSocket连接|
|[wx.onSocketOpen](https://developers.weixin.qq.com/miniprogram/dev/api/network-socket.html#wxonsocketopencallback)|监听WebSocket打开|
|[wx.onSocketError](https://developers.weixin.qq.com/miniprogram/dev/api/network-socket.html#wxonsocketerrorcallback)|监听WebSocket错误|
|[wx.sendSocketMessage](https://developers.weixin.qq.com/miniprogram/dev/api/network-socket.html#wxsendsocketmessageobject)|发送WebSocket消息|
|[wx.onSocketMessage](https://developers.weixin.qq.com/miniprogram/dev/api/network-socket.html#wxonsocketmessagecallback)|接受WebSocket消息|
|[wx.closeSocket](https://developers.weixin.qq.com/miniprogram/dev/api/network-socket.html#wxclosesocket)|关闭WebSocket连接|
|[wx.onSocketClose](https://developers.weixin.qq.com/miniprogram/dev/api/network-socket.html#wxonsocketclosecallback)|监听WebSocket关闭|
|[SocketTask](https://developers.weixin.qq.com/miniprogram/dev/api/socket-task.html)|发送数据、关闭连接、监听打开/关闭/错误/消息|
二、媒体

|API|说明|兼容|
|---|---|---|
|图片|
|[wx.chooseImage](https://developers.weixin.qq.com/miniprogram/dev/api/media-picture.html#wxchooseimageobject)|从相册选择图片或者拍照|
|[wx.priviewImage](https://developers.weixin.qq.com/miniprogram/dev/api/media-picture.html#wxpreviewimageobject)|预览图片|
|[wx.getImageInfo](https://developers.weixin.qq.com/miniprogram/dev/api/media-picture.html#wxgetimageinfoobject)|获取图片信息|
|[wx.saveImageToPhotoAlbum](https://developers.weixin.qq.com/miniprogram/dev/api/media-picture.html#wxsaveimagetophotosalbumobject)|保存图片到系统相册|基础库 1.2.0 开始支持，低版本需做兼容处理|
|录音|
|[wx.startRecord](https://developers.weixin.qq.com/miniprogram/dev/api/media-record.html#wxstartrecordobject)|开始录音|1.6.0 版本开始，本接口不再维护。建议使用能力更强的 wx.getRecorderManager 接口|
|[wx.stopRecord](https://developers.weixin.qq.com/miniprogram/dev/api/media-record.html#wxstoprecord)|结束录音|
|录音管理|
|[wx.getRecorderManager](https://developers.weixin.qq.com/miniprogram/dev/api/getRecorderManager.html)|获取 全局唯一 的录音管理器 recorderManager|基础库 1.6.0 开始支持，低版本需做兼容处理|
|音频播放控制|
|[wx.playVoice](https://developers.weixin.qq.com/miniprogram/dev/api/media-voice.html#wxplayvoice)|播放语音|1.6.0 版本开始，本接口不再维护。建议使用能力更强的 wx.createInnerAudioContext|
|[wx.pauseVoice](https://developers.weixin.qq.com/miniprogram/dev/api/media-voice.html#wxpausevoice)|暂停播放语音|
|[wx.stopVoice](https://developers.weixin.qq.com/miniprogram/dev/api/media-voice.html#wxstopvoice)|结束播放语音|
|音乐播放控制|
|[wx.getBackgroudAudioPlayerState](https://developers.weixin.qq.com/miniprogram/dev/api/media-background-audio.html#wxgetbackgroundaudioplayerstateobject)|获取音乐播放状态|v1.2.0后，API不在维护，建议使用wx.getBackgroundAudioManager|
|[wx.playBackgroundAudio](https://developers.weixin.qq.com/miniprogram/dev/api/media-background-audio.html#wxplaybackgroundaudioobject)|播放音乐|
|[wx.pauseBackgroundAudio](https://developers.weixin.qq.com/miniprogram/dev/api/media-background-audio.html#wxpausebackgroundaudio)|暂停播放音乐|
|[wx.seekBackgroundAudio](https://developers.weixin.qq.com/miniprogram/dev/api/media-background-audio.html#wxstopbackgroundaudio)|控制音乐播放进度|
|[wx.stopBackgroundAudio](https://developers.weixin.qq.com/miniprogram/dev/api/media-background-audio.html#wxstopbackgroundaudio)|停止播放音乐|
|[wx.onBackgroundAudioPlay](https://developers.weixin.qq.com/miniprogram/dev/api/media-background-audio.html#wxonbackgroundaudioplaycallback)|监听音乐开始播放|
|[wx.onBackgroundAudioPause](https://developers.weixin.qq.com/miniprogram/dev/api/media-background-audio.html#wxonbackgroundaudiopausecallback)|监听音乐暂停|
|[wx.onBackgroundAudioStop](https://developers.weixin.qq.com/miniprogram/dev/api/media-background-audio.html#wxonbackgroundaudiostopcallback)|监听音乐结束|
|背景音频播放管理|
|[wx.getBackgroundAudioManager](https://developers.weixin.qq.com/miniprogram/dev/api/getBackgroundAudioManager.html)|获取全局唯一的背景音频管理器 backgroundAudioManager|v1.2.0,低版本兼容处理|
|音频组件控制|
|[wx.createAudioContext](https://developers.weixin.qq.com/miniprogram/dev/api/api-audio.html#wxcreateaudiocontextaudioid)|创建并返回 audio 上下文 audioContext 对象|v1.6.0后该API不在维护|
|[wx.createInnerAudioContext](https://developers.weixin.qq.com/miniprogram/dev/api/createInnerAudioContext.html)|创建并返回内部 audio 上下文 innerAudioContext 对象 |基础库 1.6.0 开始支持，低版本需做兼容处理|
|视频|
|[wx.chooseVideo](https://developers.weixin.qq.com/miniprogram/dev/api/media-video.html)|从相册选择视频或拍摄|
|[wx.saveVideoToPhotosAlbum](https://developers.weixin.qq.com/miniprogram/dev/api/media-video.html#wxsavevideotophotosalbumobject)|保存视频到系统相册|v1.2.0,低版本兼容处理|
|视频组件控制|
|[wx.createVideoContext](https://developers.weixin.qq.com/miniprogram/dev/api/api-video.html#wxcreatevideocontextvideoid)|创建并返回 video 上下文 videoContext 对象||
|相机组件控制|
|[wx.createCameraContext](https://developers.weixin.qq.com/miniprogram/dev/api/api-camera.html)|创建并返回 camera 上下文 cameraContext 对象|v1.6.0,低版本兼容处理|
|实时音视频|
|[wx.createLivePlayerContext](https://developers.weixin.qq.com/miniprogram/dev/api/api-live-player.html)|操作对应的 &lt;live-player/&gt; 组件|v1.7.0,低版本兼容处理|
|[wx.createLivePusherContext](https://developers.weixin.qq.com/miniprogram/dev/api/api-live-pusher.html)|创建并返回 live-pusher 上下文 LivePusherContext 对象|v1.7.0,低版本兼容处理|
三、文件

|API|说明|兼容|
|---|---|---|
|[wx.saveFile](https://developers.weixin.qq.com/miniprogram/dev/api/file.html)|保存文件|
|[wx.getFileInfo](https://developers.weixin.qq.com/miniprogram/dev/api/getFileInfo.html)|获取文件信息|基础库 1.4.0 开始支持，低版本需做兼容处理|
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
|获取位置|
|[wx.getLocation](https://developers.weixin.qq.com/miniprogram/dev/api/location.html#wxgetlocationobject)|获取当前位置|
|[wx.chooseLocation](https://developers.weixin.qq.com/miniprogram/dev/api/location.html#wxchooselocationobject)|打开地图选择位置|
|查看位置|
|[wx.openLocation](https://developers.weixin.qq.com/miniprogram/dev/api/location.html#wxopenlocationobject)|打开内置地图|
|地图组件控制|
|[wx.createMapContext](https://developers.weixin.qq.com/miniprogram/dev/api/api-map.html#wxcreatemapcontextmapid)|地图组件控制|

六、设备

|API|说明|备注|
|---|---|---|
|系统信息|
|[wx.getSystemInfo](https://developers.weixin.qq.com/miniprogram/dev/api/systeminfo.html#wxgetsysteminfoobject)|获取系统信息|异步|
|[wx.getSystemInfoSync](https://developers.weixin.qq.com/miniprogram/dev/api/systeminfo.html#wxgetsysteminfosync)|获取系统信息|同步|
|[wx.canIUse](https://developers.weixin.qq.com/miniprogram/dev/api/api-caniuse.html)|判断小程序的API，回调，参数，组件等是否在当前版本可用|此接口从基础库 1.1.1 版本开始支持|
|网络状态|
|[wx.getNetworkType](https://developers.weixin.qq.com/miniprogram/dev/api/device.html#wxgetnetworktypeobject)|获取网络类型|
|[wx.onNetworkStatusChange](https://developers.weixin.qq.com/miniprogram/dev/api/device.html#wxonnetworkstatuschangecallback)|监听网络状态变化|基础库 1.1.0 开始支持，低版本需做兼容处理|
|加速度计|
|[wx.onAccelerometerChange](https://developers.weixin.qq.com/miniprogram/dev/api/accelerometer.html#wxonaccelerometerchangecallback)|监听加速度数据|
|[wx.startAccelerometer](https://developers.weixin.qq.com/miniprogram/dev/api/accelerometer.html#wxstartaccelerometerobject)|开始加速度数据|基础库 1.1.0 开始支持，低版本需做兼容处理|
|[wx.stopAccelerometer](https://developers.weixin.qq.com/miniprogram/dev/api/accelerometer.html#wxstopaccelerometerobject)|停止加速度数据|基础库 1.1.0 开始支持，低版本需做兼容处理|
|罗盘|
|[wx.onCompassChange](https://developers.weixin.qq.com/miniprogram/dev/api/compass.html#wxoncompasschangecallback)|监听罗盘数据|
|[wx.startCompass](https://developers.weixin.qq.com/miniprogram/dev/api/compass.html#wxstartcompassobject)|开启罗盘|基础库 1.1.0 开始支持，低版本需做兼容处理|
|[wx.stopCompass](https://developers.weixin.qq.com/miniprogram/dev/api/compass.html#wxstopcompassobject)|停止罗盘|基础库 1.1.0 开始支持，低版本需做兼容处理|
|拨打电话|
|[wx.makePhoneCall](https://developers.weixin.qq.com/miniprogram/dev/api/phonecall.html#wxmakephonecallobject)|拨打电话|
|扫码|
|[wx.scanCode](https://developers.weixin.qq.com/miniprogram/dev/api/scancode.html#wxscancodeobject)|扫码|
|剪贴板|
|[wx.setClipboardData](https://developers.weixin.qq.com/miniprogram/dev/api/clipboard.html#wxsetclipboarddataobject)|设置剪贴板内容|基础库 1.1.0 开始支持，低版本需做兼容处理|
|[wx.getClipboardData](https://developers.weixin.qq.com/miniprogram/dev/api/clipboard.html#wxsetclipboarddataobject)|获取剪贴板内容|基础库 1.1.0 开始支持，低版本需做兼容处理|
|蓝牙|基础库版本 1.1.0 开始支持，低版本需做兼容处理|iOS 微信客户端 6.5.6 版本开始支持，Android 6.5.7 版本开始支持|
|[wx.openBluetoothAdapter](https://developers.weixin.qq.com/miniprogram/dev/api/bluetooth.html#wxopenbluetoothadapterobject)|初始化小程序蓝牙模块（生命周期：从open-close或小程序销毁）|
|[wx.closeBluetoothAdapter](https://developers.weixin.qq.com/miniprogram/dev/api/bluetooth.html#wxclosebluetoothadapterobject)|关闭蓝牙模块，使其进入未初始化状态|
|[wx.getBluetoothAdapterState](https://developers.weixin.qq.com/miniprogram/dev/api/bluetooth.html#wxgetbluetoothadapterstateobject)|获取本机蓝牙适配器状态||
|[wx.onBluetoothAdapterStateChange](https://developers.weixin.qq.com/miniprogram/dev/api/bluetooth.html#wxonbluetoothadapterstatechangecallback)|监听蓝牙适配器状态变化事件||
|[wx.startBluetoothDevicesDiscovery](https://developers.weixin.qq.com/miniprogram/dev/api/bluetooth.html#wxstartbluetoothdevicesdiscoveryobject)|开始搜寻附近的蓝牙外围设备||
|[wx.stopBluetoothDevicesDiscovery](https://developers.weixin.qq.com/miniprogram/dev/api/bluetooth.html#wxstopbluetoothdevicesdiscoveryobject)|停止搜寻附近的蓝牙外围设备||
|[wx.getBluetoothDevices](https://developers.weixin.qq.com/miniprogram/dev/api/bluetooth.html#wxgetbluetoothdevicesobject)|获取在小程序蓝牙模块生效期间所有已发现的蓝牙设备，包括已经和本机处于连接状态的设备||
|[wx.getConnectedBluetoothDevices](https://developers.weixin.qq.com/miniprogram/dev/api/bluetooth.html#wxgetconnectedbluetoothdevicesobject)|根据 uuid 获取处于已连接状态的设备||
|[wx.onBluetoothDeviceFound](https://developers.weixin.qq.com/miniprogram/dev/api/bluetooth.html#wxonbluetoothdevicefoundcallback)|监听寻找到新设备的事件||
|[wx.createBLEConnection](https://developers.weixin.qq.com/miniprogram/dev/api/bluetooth.html#wxcreatebleconnectionobject)|连接低功耗蓝牙设备||
|[wx.closeBLEConnection](https://developers.weixin.qq.com/miniprogram/dev/api/bluetooth.html#wxclosebleconnectionobject)|断开与低功耗蓝牙设备的连接||
|[wx.getBLEDeviceServices](https://developers.weixin.qq.com/miniprogram/dev/api/bluetooth.html#wxgetbledeviceservicesobject)|获取蓝牙设备所有 service（服务）||
|[wx.getBLEDeviceCharacteristics](https://developers.weixin.qq.com/miniprogram/dev/api/bluetooth.html#wxgetbledevicecharacteristicsobject)|获取蓝牙设备某个服务中的所有 characteristic（特征值）||
|[wx.readBLECharacteristicValue](https://developers.weixin.qq.com/miniprogram/dev/api/bluetooth.html#wxreadblecharacteristicvalueobject)|读取低功耗蓝牙设备的特征值的二进制数据值||
|[wx.writeBLECharacteristicValue](https://developers.weixin.qq.com/miniprogram/dev/api/bluetooth.html#wxwriteblecharacteristicvalueobject)|向低功耗蓝牙设备特征值中写入二进制数据||
|[wx.notifyBLECharacteristicValueChange](https://developers.weixin.qq.com/miniprogram/dev/api/bluetooth.html#wxnotifyblecharacteristicvaluechangeobject)|启用低功耗蓝牙设备特征值变化时的 notify 功能，订阅特征值||
|[wx.onBLEConnectionStateChange](https://developers.weixin.qq.com/miniprogram/dev/api/bluetooth.html#wxonbleconnectionstatechangecallback)|监听低功耗蓝牙连接状态的改变事件，包括开发者主动连接或断开连接，设备丢失，连接异常断开等等||
|[wx.onBLECharacteristicValueChange](https://developers.weixin.qq.com/miniprogram/dev/api/bluetooth.html#wxonblecharacteristicvaluechangecallback)|监听低功耗蓝牙设备的特征值变化。必须先启用notify接口才能接收到设备推送的notification||
|[错误码](https://developers.weixin.qq.com/miniprogram/dev/api/bluetooth.html#%E8%93%9D%E7%89%99%E9%94%99%E8%AF%AF%E7%A0%81errcode%E5%88%97%E8%A1%A8)|蓝牙错误码列表||
|iBeacon|
|[wx.startBeaconDiscovery](https://developers.weixin.qq.com/miniprogram/dev/api/iBeacon.html#wxstartbeacondiscoveryobject)|开始搜索附近的iBeacon设备|基础库 1.2.0 开始支持，低版本需做兼容处理|
|[wx.stopBeaconDiscovery](https://developers.weixin.qq.com/miniprogram/dev/api/iBeacon.html#wxstopbeacondiscoveryobject)|停止搜索附近的iBeacon设备||
|[wx.getBeacons](https://developers.weixin.qq.com/miniprogram/dev/api/iBeacon.html#wxgetbeaconsobject)|获取所有已搜索到的iBeacon设备||
|[wx.onBeaconUpdate](https://developers.weixin.qq.com/miniprogram/dev/api/iBeacon.html#wxonbeaconupdatecallback)|监听 iBeacon 设备的更新事件||
|[wx.onBeaconServiceChange](https://developers.weixin.qq.com/miniprogram/dev/api/iBeacon.html#wxonbeaconservicechangecallback)|监听 iBeacon 服务的状态变化||
|屏幕亮度|
|[wx.setScreenBrightness](https://developers.weixin.qq.com/miniprogram/dev/api/device.html#wxsetscreenbrightnessobject)|设置屏幕亮度|基础库 1.2.0 开始支持，低版本需做兼容处理|
|[wx.getScreenBrightness](https://developers.weixin.qq.com/miniprogram/dev/api/device.html#wxgetscreenbrightnessobject)|获取屏幕亮度||
|[wx.setKeepScreenOn](https://developers.weixin.qq.com/miniprogram/dev/api/setKeepScreenOn.html)|设置是否保持常亮状态||
|用户截屏事件|
|[wx.onUserCaptureScreen](https://developers.weixin.qq.com/miniprogram/dev/api/onUserCaptureScreen.html)|监听用户主动截屏事件，用户使用系统截屏按键截屏时触发此事件||
|振动|
|[wx.vibrateLong](https://developers.weixin.qq.com/miniprogram/dev/api/device.html#wxvibratelongobject)|使手机发生较长时间的振动（400ms）|基础库 1.2.0 开始支持，低版本需做兼容处理|
|[wx.vibrateShort](https://developers.weixin.qq.com/miniprogram/dev/api/device.html#wxvibrateshortobject)|使手机发生较短时间的振动（15ms）||
|手机联系人|
|[wx.addPhoneContact](https://developers.weixin.qq.com/miniprogram/dev/api/phone-contact.html#wxaddphonecontactobject)|调用后，用户可以选择将该表单以“新增联系人”或“添加到已有联系人”的方式，写入手机系统通讯录，完成手机通讯录联系人和联系方式的增加||
|NFC|
|[wx.getHCEState](https://developers.weixin.qq.com/miniprogram/dev/api/nfc.html#wxgethcestateobject)|判断当前设备是否支持 HCE 能力|基础库 1.7.0 开始支持，低版本需做兼容处理|
|[wx.startHCE](https://developers.weixin.qq.com/miniprogram/dev/api/nfc.html#wxstarthceobject)|初始化 NFC 模块||
|[wx.stopHCE](https://developers.weixin.qq.com/miniprogram/dev/api/nfc.html#wxstophceobject)|关闭 NFC 模块。仅在安卓系统下有效||
|[wx.onHCEMessag](https://developers.weixin.qq.com/miniprogram/dev/api/nfc.html#wxonhcemessagecallback)|监听 NFC 设备的消息回调，并在回调中处理||
|[wx.sendHCEMessage](https://developers.weixin.qq.com/miniprogram/dev/api/nfc.html#wx.sendhcemessageobject)|发送 NFC 消息。仅在安卓系统下有效||
|WiFi|
|[wx.startWifi](https://developers.weixin.qq.com/miniprogram/dev/api/wifi.html#wxstartwifiobject)|初始化 Wi-Fi 模块|基础库 1.6.0 开始支持，低版本需做兼容处理|
|[wx.stopWifi](https://developers.weixin.qq.com/miniprogram/dev/api/wifi.html#wxstopwifiobject)|关闭 Wi-Fi 模块||
|[wx.connectWifi](https://developers.weixin.qq.com/miniprogram/dev/api/wifi.html#wxconnectwifiobject)|连接 Wi-Fi。若已知 Wi-Fi 信息，可以直接利用该接口连接。|仅 Android 与 iOS 11 以上版本支持|
|[wx.getWifiList](https://developers.weixin.qq.com/miniprogram/dev/api/wifi.html#wxgetwifilistobject)|请求获取 Wi-Fi 列表，在 onGetWifiList 注册的回调中返回 wifiList 数据||
|[wx.onGetWifiList](https://developers.weixin.qq.com/miniprogram/dev/api/wifi.html#wxongetwifilistcallback)|监听在获取到 Wi-Fi 列表数据时的事件，在回调中将返回 wifiList||
|[wx.setWifiList](https://developers.weixin.qq.com/miniprogram/dev/api/wifi.html#wxsetwifilistobject)|iOS特有接口 在 onGetWifiList 回调后，利用接口设置 wifiList 中 AP 的相关信息||
|[wx.onWifiConnected](https://developers.weixin.qq.com/miniprogram/dev/api/wifi.html#wxonwificonnectedcallback)|监听连接上 Wi-Fi 的事件||
|[wx.getConnectedWifi](https://developers.weixin.qq.com/miniprogram/dev/api/wifi.html#wxgetconnectedwifiobject)|获取已连接中的 Wi-Fi 信息||
七、界面

|API|说明|备注|
|---|---|---|
|交互反馈|
|[wx.showToast](https://developers.weixin.qq.com/miniprogram/dev/api/api-react.html#wxshowtoastobject)|显示提示框|
|[wx.showLoading](https://developers.weixin.qq.com/miniprogram/dev/api/api-react.html#wxshowloadingobject)|显示加载提示框|
|[wx.hideToast](https://developers.weixin.qq.com/miniprogram/dev/api/api-react.html#wxhidetoast)|隐藏提示框|
|[wx.hideLoading](https://developers.weixin.qq.com/miniprogram/dev/api/api-react.html#wxhideloading)|隐藏加载提示框|
|[wx.showModal](https://developers.weixin.qq.com/miniprogram/dev/api/api-react.html#wxshowmodalobject)|显示模态弹窗|
|[wx.showActionSheet](https://developers.weixin.qq.com/miniprogram/dev/api/api-react.html#wxshowactionsheetobject)|显示菜单列表|
|设置导航条|
|[wx.setNavigationBarTitle](https://developers.weixin.qq.com/miniprogram/dev/api/ui.html#wxsetnavigationbartitleobject)|设置当前页面标题|
|[wx.showNavigationBarLoading](https://developers.weixin.qq.com/miniprogram/dev/api/ui.html#wxshownavigationbarloading)|显示导航条加载动画|
|[wx.hideNavigationBarLoading](https://developers.weixin.qq.com/miniprogram/dev/api/ui.html#wxhidenavigationbarloading)|隐藏导航条加载动画|
|[wx.setNavigationBarColor](https://developers.weixin.qq.com/miniprogram/dev/api/setNavigationBarColor.html)|设置导航条颜色|
|设置tabBar|基础库 1.9.0 开始支持，低版本需做兼容处理|
|[wx.setTabBarBadge](https://developers.weixin.qq.com/miniprogram/dev/api/ui-tabbar.html#wxsettabbarbadgeobject)|为 tabBar 某一项的右上角添加文本||
|[wx.removeTabBarBadge](https://developers.weixin.qq.com/miniprogram/dev/api/ui-tabbar.html#wxremovetabbarbadgeobject)|移除 tabBar 某一项右上角的文本||
|[wx.showTabBarRedDot](https://developers.weixin.qq.com/miniprogram/dev/api/ui-tabbar.html#wxshowtabbarreddotobject)|显示 tabBar 某一项的右上角的红点||
|[wx.hideTabBarRedDot](https://developers.weixin.qq.com/miniprogram/dev/api/ui-tabbar.html#wxhidetabbarreddotobject)|隐藏 tabBar 某一项的右上角的红点||
|[wx.setTabBarStyle](https://developers.weixin.qq.com/miniprogram/dev/api/ui-tabbar.html#wxsettabbarstyleobject)|动态设置 tabBar 的整体样式||
|[wx.setTabBarItem](https://developers.weixin.qq.com/miniprogram/dev/api/ui-tabbar.html#wxsettabbaritemobject)|动态设置 tabBar 某一项的内容||
|[wx.showTabBar](https://developers.weixin.qq.com/miniprogram/dev/api/ui-tabbar.html#wxshowtabbarobject)|显示 tabBar||
|[wx.hideTabBar](https://developers.weixin.qq.com/miniprogram/dev/api/ui-tabbar.html#wxhidetabbarobject)|隐藏 tabBar||
|设置置顶信息|
|[wx.setTopBarText](https://developers.weixin.qq.com/miniprogram/dev/api/ui.html#wxsettopbartextobject)|动态设置置顶栏文字内容，只有当前小程序被置顶时能生效，如果当前小程序没有被置顶，也能调用成功，但是不会立即生效，只有在用户将这个小程序置顶后才换上设置的文字内容|调用成功后，需间隔 5s 才能再次调用此接口，如果在 5s 内再次调用此接口，会回调 fail|
|导航|
|[wx.navigateTo](https://developers.weixin.qq.com/miniprogram/dev/api/ui-navigate.html#wxnavigatetoobject)|新窗口打开页面|
|[wx.redirectTo](https://developers.weixin.qq.com/miniprogram/dev/api/ui-navigate.html#wxredirecttoobject)|原窗口打开页面|
|[wx.switchTab](https://developers.weixin.qq.com/miniprogram/dev/api/ui-navigate.html#wxswitchtabobject)|切换到tabbar页面|
|[wx.navigateBack](https://developers.weixin.qq.com/miniprogram/dev/api/ui-navigate.html#wxnavigateback)|退回上一个页面|
|[wx.reLaunch](https://developers.weixin.qq.com/miniprogram/dev/api/ui-navigate.html#wxrelaunchobject)|关闭所有页面，打开到应用内的某个页面||
|动画|
|[wx.createAnimation](https://developers.weixin.qq.com/miniprogram/dev/api/api-animation.html)|动画|
|位置|
|[wx.pageScrollTo](https://developers.weixin.qq.com/miniprogram/dev/api/scroll.html)|将页面滚动到目标位置||
|绘图|
|[intro](https://developers.weixin.qq.com/miniprogram/dev/api/canvas/intro.html)|在Canvas上画图||
|[coordinates](https://developers.weixin.qq.com/miniprogram/dev/api/canvas/coordinates.html)|Canvas坐标系||
|[gradient](https://developers.weixin.qq.com/miniprogram/dev/api/canvas/gradient.html)|渐变||
|[Reference](https://developers.weixin.qq.com/miniprogram/dev/api/canvas/reference.html)|API||
|[color](https://developers.weixin.qq.com/miniprogram/dev/api/canvas/color.html)|颜色的使用||
|[wx.createCanvasContext](https://developers.weixin.qq.com/miniprogram/dev/api/canvas/create-canvas-context.html)|创建 canvas 绘图上下文（指定 canvasId)||
|[wx.createContext](https://developers.weixin.qq.com/miniprogram/dev/api/canvas/create-canvas-context.html)|创建绘图上下文|
|[wx.drawCanvas](https://developers.weixin.qq.com/miniprogram/dev/api/canvas/draw-canvas.html)|绘图|
|[wx.canvasToTempFilePath](https://developers.weixin.qq.com/miniprogram/dev/api/canvas/temp-file.html)|把当前画布指定区域的内容导出生成指定大小的图片，并返回文件路径||
|[wx.canvasGetImageData](https://developers.weixin.qq.com/miniprogram/dev/api/canvas/get-image-data.html)|返回一个数组，用来描述 canvas 区域隐含的像素数据||
|[wx.canvasPutImageData](https://developers.weixin.qq.com/miniprogram/dev/api/canvas/put-image-data.html)|将像素数据绘制到画布的方法||
|[canvasContext.setFillStyle](https://developers.weixin.qq.com/miniprogram/dev/api/canvas/set-fill-style.html)|设置填充色(默认black)||
|[canvasContext.setStrokeStyle](https://developers.weixin.qq.com/miniprogram/dev/api/canvas/set-stroke-style.html)|设置边框颜色(默认black)||
|[canvasContext.setShadow](https://developers.weixin.qq.com/miniprogram/dev/api/canvas/set-shadow.html)|设置阴影样式(offsetX 默认值为0， offsetY 默认值为0， blur 默认值为0，color 默认值为 black)||
|[canvasContext.createLinearGradient](https://developers.weixin.qq.com/miniprogram/dev/api/canvas/create-linear-gradient.html)|创建一个线性的渐变颜色(需要使用 addColorStop() 来指定渐变点，至少要两个)||
|[canvasContext.createCircularGradient](https://developers.weixin.qq.com/miniprogram/dev/api/canvas/create-circular-gradient.html)|创建一个圆形的渐变颜色(起点在圆心，终点在圆环, 需要使用 addColorStop() 来指定渐变点，至少要两个)||
|[canvasContext.addColorStop](https://developers.weixin.qq.com/miniprogram/dev/api/canvas/add-color-stop.html)|创建一个颜色的渐变点||
|...|...||
|下拉刷新||基础库 1.5.0 开始支持，低版本需做兼容处理|
|[Page.onPullDownRefresh](https://developers.weixin.qq.com/miniprogram/dev/api/pulldown.html#onpulldownrefresh)|在 Page 中定义 onPullDownRefresh 处理函数，监听该页面用户下拉刷新事件||
|[wx.startPullDownRefresh](https://developers.weixin.qq.com/miniprogram/dev/api/pulldown.html#wxstartpulldownrefresh)|开始下拉刷新，调用后触发下拉刷新动画，效果与用户手动下拉刷新一致||
|[wx.stopPullDownRefresh](https://developers.weixin.qq.com/miniprogram/dev/api/pulldown.html#wxstoppulldownrefresh)|停止下拉刷新动画|
|WXML节点信息|
|[wx.createSelectorQuery](https://developers.weixin.qq.com/miniprogram/dev/api/wxml-nodes-info.html#wxcreateselectorquery)|返回一个SelectorQuery对象实例||
|[selectorQuery.in](https://developers.weixin.qq.com/miniprogram/dev/api/wxml-nodes-info.html#selectorqueryincomponent)|将选择器的选取范围更改为自定义组件component内||
|[selectorQuery.select](https://developers.weixin.qq.com/miniprogram/dev/api/wxml-nodes-info.html#selectorqueryselectselector)|返回一个NodesRef对象实例，可以用于获取节点信息||
|[selectorQuery.selectAll](https://developers.weixin.qq.com/miniprogram/dev/api/wxml-nodes-info.html#selectorqueryselectallselector)|在当前页面下选择匹配选择器selector的节点，返回一个NodesRef对象实例||
|[selectorQuery.selectViewport](https://developers.weixin.qq.com/miniprogram/dev/api/wxml-nodes-info.html#selectorqueryselectviewport)|选择显示区域，可用于获取显示区域的尺寸、滚动位置等信息，返回一个NodesRef对象实例||
|[nodesRef.boundingClientRect](https://developers.weixin.qq.com/miniprogram/dev/api/wxml-nodes-info.html#selectorqueryselectviewport)|添加节点的布局位置的查询请求，相对于显示区域，以像素为单位||
|[nodesRef.scrollOffset](https://developers.weixin.qq.com/miniprogram/dev/api/wxml-nodes-info.html#nodesrefscrolloffsetcallback)|添加节点的滚动位置查询请求，以像素为单位||
|[nodesRef.fields](https://developers.weixin.qq.com/miniprogram/dev/api/wxml-nodes-info.html#nodesreffieldsfieldscallback)|获取节点的相关信息，需要获取的字段在fields中指定||
|[selectorQuery.exec](https://developers.weixin.qq.com/miniprogram/dev/api/wxml-nodes-info.html#selectorqueryexeccallback)|执行所有的请求，请求结果按请求次序构成数组，在callback的第一个参数中返回||
|WXML节点布局相交状态|
|[wx.createIntersectionObserver](https://developers.weixin.qq.com/miniprogram/dev/api/intersection-observer.html#intersectionobserverobservetargetselectorcallback)|创建并返回一个 IntersectionObserver 对象实例||
|[intersectionObserver.relativeTo]()|使用选择器指定一个节点，作为参照区域之一||
|[intersectionObserver.relativeToViewport]()|指定页面显示区域作为参照区域之一||
|[intersectionObserver.observe]()|指定目标节点并开始监听相交状态变化情况||
|[intersectionObserver.disconnect]()|停止监听。回调函数将不再触发||

八、第三方平台

|API|说明|
|---|---|
|[wx.getExtConfig]()|获取[第三方平台](https://developers.weixin.qq.com/miniprogram/dev/devtools/ext.html)自定义的数据字段|
|[wx.getExtConfigSync](https://developers.weixin.qq.com/miniprogram/dev/api/ext-api.html#wxgetextconfigsync)|获取第三方平台自定义的数据字段的同步接口|
九、开放接口

|API|说明|
|---|---|
|登录||
|[wx.login](https://developers.weixin.qq.com/miniprogram/dev/api/api-login.html)|登录|
|[wx.checkSession](https://developers.weixin.qq.com/miniprogram/dev/api/signature.html#wxchecksessionobject)|校验用户当前session_key是否有效|
|[签名加密](https://developers.weixin.qq.com/miniprogram/dev/api/signature.html)||
|授权||
|[wx.authorize](https://developers.weixin.qq.com/miniprogram/dev/api/authorize.html)|提前向用户发起授权请求|
|用户信息||
|[wx.getUserInfo](https://developers.weixin.qq.com/miniprogram/dev/api/open.html#wxgetuserinfoobject)|获取用户信息|
|[getPhoneNumber](https://developers.weixin.qq.com/miniprogram/dev/api/getPhoneNumber.html)|获取微信用户绑定的手机号，需先调用login接口|
|[UnionID机制说明](https://developers.weixin.qq.com/miniprogram/dev/api/unionID.html)||
|微信支付||
|[wx.requestPayment](https://developers.weixin.qq.com/miniprogram/dev/api/api-pay.html#wxrequestpaymentobject)|发起微信支付|
|模板消息||
|[使用说明](https://developers.weixin.qq.com/miniprogram/dev/api/notice.html#使用说明)|eg:购买成功通知，续费成功通知，预定成功通知，等模板|
|[模版消息管理](https://developers.weixin.qq.com/miniprogram/dev/api/notice.html#模版消息管理)||
|[发送模板消息](https://developers.weixin.qq.com/miniprogram/dev/api/notice.html#发送模板消息)||
|客服消息||
|[接受消息和事件](https://developers.weixin.qq.com/miniprogram/dev/api/custommsg/receive.html#接收消息和事件)||
|[发送客服消息]()||
|[转发消息]()||
|[临时素材接口]()||
|[客服输入状态]()||
|[接入指引]()||
|转发||
|[Page.onShareAppMessage](https://developers.weixin.qq.com/miniprogram/dev/api/share.html#onshareappmessageoptions)|在 Page 中定义 onShareAppMessage 函数，设置该页面的转发信息|
|[wx.showShareMenu]()|显示当前页面的转发按钮|
|[wx.hideShareMenu]()|隐藏转发按钮|
|[wx.updateShareMenu](https://developers.weixin.qq.com/miniprogram/dev/api/share.html#wxupdatesharemenuobject)|更新转发属性|
|[wx.getShareInfo](https://developers.weixin.qq.com/miniprogram/dev/api/share.html#wxgetshareinfoobject)|获取转发详细信息|
|[获取更多转发信息]()||
|[页面内发起转发]()||
|获取二维码||
|[获取二维码](https://developers.weixin.qq.com/miniprogram/dev/api/qrcode.html)||
|收货地址||
|[wx.chooseAddress](https://developers.weixin.qq.com/miniprogram/dev/api/address.html#wxchooseaddressobject)|获取用户收货地址|
|卡券||
|[wx.addCard](https://developers.weixin.qq.com/miniprogram/dev/api/card.html#wxaddcardobject)|添加卡券|
|[wx.openCard](https://developers.weixin.qq.com/miniprogram/dev/api/card.html#wxopencardobject)|打开卡券|
|[会员卡组件](https://developers.weixin.qq.com/miniprogram/dev/api/card.html#会员卡组件)||
|设置||
|[wx.openSetting](https://developers.weixin.qq.com/miniprogram/dev/api/setting.html#wxopensettingobject)|调起客户端小程序设置界面，返回用户设置的操作结果|
|[wx.getSetting](https://developers.weixin.qq.com/miniprogram/dev/api/setting.html#wxgetsettingobject)|获取用户的当前设置|
|微信运动||
|[wx.getWeRunData](https://developers.weixin.qq.com/miniprogram/dev/api/we-run.html#wxgetwerundataobject)|获取用户过去三十天微信运动步数，需要先调用 wx.login 接口|
|打开小程序||
|[wx.navigateToMiniProgram](https://developers.weixin.qq.com/miniprogram/dev/api/navigateToMiniProgram.html)|打开同一公众号下关联的另一个小程序|
|[wx.navigateBackMiniProgram](https://developers.weixin.qq.com/miniprogram/dev/api/navigateBackMiniProgram.html)|返回到上一个小程序，只有在当前小程序是被其他小程序打开时可以调用成功|
|打开APP||
|[launchApp](https://developers.weixin.qq.com/miniprogram/dev/api/launchApp.html)|因为需要用户主动触发才能打开 APP，所以该功能不由 API 来调用|
|获取发票抬头||
|[wx.chooseInvoiceTitle](https://developers.weixin.qq.com/miniprogram/dev/api/chooseInvoiceTitle.html)|选择用户的发票抬头|
|生物认证||
|[wx.checkIsSupportSoterAuthentication](https://developers.weixin.qq.com/miniprogram/dev/api/checkIsSupportSoterAuthentication.html)|获取本机支持的 SOTER 生物认证方式|
|[wx.startSoterAuthentication](https://developers.weixin.qq.com/miniprogram/dev/api/startSoterAuthentication.html)|开始 SOTER 生物认证|
|[wx.checkIsSoterEnrolledInDevice](https://developers.weixin.qq.com/miniprogram/dev/api/checkIsSoterEnrolledInDevice.html)|获取设备内是否录入如指纹等生物信息的接口|
|附近||
|[添加地点](https://developers.weixin.qq.com/miniprogram/dev/api/nearby.html#添加地点)||
|[查看地点列表](https://developers.weixin.qq.com/miniprogram/dev/api/nearby.html#查看地点列表)||
|[删除地点](https://developers.weixin.qq.com/miniprogram/dev/api/nearby.html#删除地点)||
|[展示/取消展示附近小程序](https://developers.weixin.qq.com/miniprogram/dev/api/nearby.html#展示取消展示附近小程序)||
|插件管理||
|[插件管理](https://developers.weixin.qq.com/miniprogram/dev/api/plugin.html)||
十、数据

|API|说明|
|---|---|
|常规分析||
|[概括趋势](https://developers.weixin.qq.com/miniprogram/dev/api/analysis.html#概况趋势)||
|[访问趋势](https://developers.weixin.qq.com/miniprogram/dev/api/analysis-visit.html#访问趋势)||
|[访问分布](https://developers.weixin.qq.com/miniprogram/dev/api/analysis-visit.html#访问分布)||
|[访问留存](https://developers.weixin.qq.com/miniprogram/dev/api/analysis-visit.html#访问留存)||
|[访问页面](https://developers.weixin.qq.com/miniprogram/dev/api/analysis-visit.html#访问页面)||
|自定义分析||
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

