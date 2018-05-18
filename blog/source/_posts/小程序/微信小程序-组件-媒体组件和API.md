---
title: 微信小程序-组件-媒体组件和API
date: 2018-05-15 13:22:48
tags: 微信小程序
categories: 小程序
---

一、Audio组件和API		
1.属性	
	
|属性名|类型|默认值|说明|
|---|---|---|---|
|id|String||audio组件的唯一标志符|	
|src|String||音频资源地址|
|loop|Boolean||是否循环播放|
|controls| Boolean ||是否显示默认控件|
|poster| String ||默认控件上音频封面图片资源地址(controls=false,poster无效)|
|name| String |未知音频|默认控件上的音频名字(controls=false,poster无效)|
|author| String |未知作者|默认控件上的作者名字(controls=false,poster无效)|
|binderror|EvenHandle||当发生错误时触发 error 事件，detail = {errMsg: MediaError.code}|
|bindplay| EvenHandle ||当开始/继续播放时触发play事件|
|bindpause| EvenHandle ||当暂停播放时触发 pause 事件|
|bindtimeupdate| EvenHandle ||当播放进度改变时触发 timeupdate 事件，detail = {currentTime, duration}|
|bindended| EvenHandle ||当播放到末尾时触发 ended 事件|
				
2.错误码 MediaError.code
	
	1 - 获取资源被用户禁止
	2 - 网络错误
	3 - 解码错误
	4 - 不合适资源	
3.音频组件API

推荐V1.6.0+ wx.createInnerAudioContext() 

|方法|描述|只读|
|---|---|---|
|src|String|音频地址，用于直接播放||
|startTime|Number|开始播放位置，默认0，单位s||
|autoPlay|Boolean|默认false||
|loop| Boolean |是否循环播放，默认false||
|obeyMyteSwitch| Boolean |是否遵循系统静音开关，false即使用户打开静音开关也能发出声音，默认true|
|duration| Number |当前音频时长（s），src合法返回|
|currentTime| Number |当前音频的播放位置（单位：s），只有在当前有合法的 src 时返回，时间不取整，保留小数点后 6 位|
|paused| Boolean |当前是是否暂停或停止状态，true 表示暂停或停止，false 表示正在播放|
|buffered|Number|音频缓冲的时间点，仅保证当前播放时间点到此时间点内容已缓冲。|
|volume|Number|音量。范围 0~1|

innerAudioContext 对象的方法列表：

|方法|参数|说明|
|---|---|---|
|play	|无|	播放	|
|pause	|无	|暂停	|
|stop	|无|	停止	|
|seek	|position	|跳转到指定位置，单位 s	|
|destroy	|无|	销毁当前实例|	
|onCanplay	|callback	|音频进入可以播放状态，但不保证后面可以流畅播放	|
|onPlay|	callback	|音频播放事件	|
|onPause|	callback	|音频暂停事件	|
|onStop	|callback	|音频停止事件	|
|onEnded	|callback	|音频自然播放结束事件	|
|onTimeUpdate |	callback|	音频播放进度更新事件|	
|onError	|callback	| 音频播放错误事件	|
|onWaiting|	callback|	音频加载中事件，当音频因为数据不足，需要停下来加载时会触发	|
|onSeeking	|callback|	音频进行 seek 操作事件	|
|onSeeked|	callback	|音频完成 seek 操作事件	|
|offCanplay|	callback|	取消监听 onCanplay 事件	1.9.0|
|offPlay	|callback	|取消监听 onPlay 事件	1.9.0|
|offPause	|callback	|取消监听 onPause 事件	1.9.0|
|offStop	|callback	|取消监听 onStop 事件	1.9.0|
|offEnded	|callback|	取消监听 onEnded 事件	1.9.0|
|offTimeUpdate	| callback	|取消监听 onTimeUpdate 事件	1.9.0|
|offError	|callback	| 取消监听 onError 事件	1.9.0|
|offWaiting|	callback	|取消监听 onWaiting 事件	1.9.0|
|offSeeking|	callback|	取消监听 onSeeking 事件	1.9.0|
|offSeeked|	callback|	取消监听 onSeeked 事件|

errCode 说明

	10001 		系统错误
	10002 		网络错误
	10003 		文件错误
	10004		格式错误
	-1			未知错误
不推荐V1.6.0-  wx.createAudioContext(audioId, this) 

|方法|参数|描述|
|---|---|---|
|setSrc|src|音频地址|
|play||播放|
|pause||暂停|
|seek|position|跳转指定位置，单位s|
4.eg		
![audio](audio.png)		

```html
parents.wxml

<view class='myAudio'>
	<audio 
		poster='https://tao1010.github.io/image/avatar.gif' 
		name='此时此刻' 
		author='许巍' 
		src='http://ws.stream.qqmusic.qq.com/M500001VfvsJ21xFqb.mp3?guid=ffffffff82def4af4b12b3cd9337d5e7&uin=346897220&vkey=6292F51E1E384E06DCBDC9AB7C49FD713D632D313AC4858BACB8DDD29067D3C601481D36E62053BF8DFEAF74C0A5CCFADD6471160CAF3E6A&fromtag=46' 
		id='audio' 
		controls='true' 
		loop='true'>
	</audio>

	<slider 
		step='1' 
		bindchange='quickPlay' 
		value='{{value}}' 
		min='0' 
		max='{{max}}' 
		show-value='true'></slider>
	
	<button type='primary' bindtap='audioPlay'>播放</button>
	<button type='primary' bindtap='audioPause'>暂停</button>
	<button type='primary' bindtap='audioStart'>重新播放</button>
</view>
```
```js
parents.js

quickPlay: function(e){

    console.log(e.detail.value)
    this.audioCtx.seek(e.detail.value)
  },
  audioPlay: function(){

    this.audioCtx.play()
  },
  audioPause: function(){
    this.audioCtx.pause()
  },
  audioStart: function(){
    this.audioCtx.seek(0)
  },
  /**
   * 生命周期函数--监听页面加载
   */
  onLoad: function (options) {

    // template.tabar("tabBar",4,this)//0 表示第一个tabbar
    // 使用 wx.createAudioContext 获取 audio 上下文 context
    this.audioCtx = wx.createAudioContext("audio", this)
    // this.audioCtx = wx.createInnerAudioContext();
    max = this.audioCtx.detail.duration;
    // console.log(this.audioCtx.detail);
  },

```
二、Video组件和API		
1.[组件属性](https://developers.weixin.qq.com/miniprogram/dev/component/video.html)	

	 <view>
	  <view class='title'>1-402前</view>
	  <video class='video1' src='{{videoSrcFront}}'>视频监控</video>
	</view>
	<view>
	  <view class='title'>2-205后</view>
	  <video class='video2' src='{{videoSrc}}' controls='true' danmu-btn='true' enable-danmu='true' danmu-list='{{list}}'>视频监控</video>
	</view> 
2.注意
	
	video组件是由客户端创建的原生组件，层级最高，不能通过z-index控制层级；
	不能在scroll-view、swiper、picker-view、movable-view中使用；
	css动画在video组件中无效；
	默认宽度300px、高度225px，可通过wxss设置宽高；
3.API
	
	wx.chooseVideo 拍摄视频或从相册中选视频，返回视频的临时文件路径
	wx.saveVideoToPhotosAlbum 保存视频到系统相册，需要用户授权scope.writePhotosAlbum
	wx.createVideoCOntext(videoId,this) 创建并返回video上下文videoContext对象
三、camera组件和API		
1.属性		

|属性名|类型|默认值|描述|
|---|---|---|---|
|device-postion|String|back|前置或后置,值为front,back|
|flash|String|auto|闪关灯 值为auto,on,off|
|bindstop| EventHandle ||摄像头在非正常终止时触发，如退出后台等情况|
|binderror|EventHandle||用户不允许使用摄像头时触发|
2.注意

	camera组件是由客户端创建的原生组件，层级最高，不能通过z-index控制层级,可使用cover-view、cover-image覆盖在上面；
	不能在scroll-view、swiper、picker-view、movable-view中使用；
	同一页面只能插入一个camera组件；	
	
3.API	
	
	wx.createCameraContext(this) 创建并返回 camera 上下文 cameraContext 对象，cameraContext 与页面的 camera 组件绑定，一个页面只能有一个camera，通过它可以操作对应的 <camera/> 组件;
		cameraContext对象方法：
		takePhoto - 拍照
		startRecord - 开始录像
		stopRecord - 结束录像
4.eg

```html

 <view>  
    <camera device-position='back' flash='off' binderror='error' style='width:100%; height: 300px;'></camera>
    <button type='default' bindtap='takePhoto'>拍照</button>
    <view>预览</view>
    <image mode='widthFix' src='{{src}}'></image>
  </view>
 js
 
takePhoto: function(){
const ctx = wx.createCameraContext()
// 拍照
ctx.takePhoto({
  quality: 'high',
  success: (res) => {
    this.setData({
      src: res.tempImagePath
    })
  }
})
},
error(e) {

console.log(e.detail)
},
   
```
四、image组件和API			
1.属性

|属性名|类型|默认值|描述|
|---|---|---|---|
|src|String||图片资源地址|
|mode|String|‘scaleToFill’|图片裁剪、缩放的模式|
|lazy-load|Boolean|false|图片懒加载。针对page与scroll-view下的image有效|
|binderror| HandleEvent ||当错误发生时，发布到 AppService 的事件名，事件对象event.detail = {errMsg: 'something wrong'}|
|bindload| HandleEvent ||当图片载入完毕时，发布到 AppService 的事件名，事件对象event.detail = {height:'图片高度px', width:'图片宽度px'}|
|mode值|||mode13种模式(4中缩放，9种裁剪)|
|缩放|scaleToFill||不保持纵横比缩放图片，使图片的宽高完全拉伸至填满 image 元素|
|缩放|aspectFit||保持纵横比缩放图片，使图片的长边能完全显示出来。也就是说，可以完整地将图片显示出来。|
|缩放|aspectFill||保持纵横比缩放图片，只保证图片的短边能完全显示出来.|
|缩放|widthFix||宽度不变，高度自动变化，保持原图宽高比不变|
|裁剪|top||...顶部...|
|裁剪|bottom||...底部...|
|裁剪|center||...中间...|
|裁剪|left||...左边...|
|裁剪|right||...右边...|
|裁剪|top left||...左上...|
|裁剪|top right||...右上...|
|裁剪|bottom let||...左下...|
|裁剪|bottom right||...右下...|
2.注意
	
	image默认组件width=300px，height=225px;
	文件的临时路径，在小程序本次启动期间可以正常使用，如需持久保存，需在主动调用 wx.saveFile，在小程序下次启动时才能访问得到;
	
3.API
	
	wx.chooseImage 从相册选择图片或使用相机拍照；
	wx.previewImage 预览图片；
	wx.getImageInfo 获取图片信息；
	wx.saveImageToPhotosAlbum 保存图片到系统相册；
4.eg

```html
<image class='background' style="width: 100%; height: 100%;" src='../../../images/bg.png'></image>

js
wx.chooseImage({
  count: 1, // 默认9
  sizeType: ['original', 'compressed'], // 可以指定是原图还是压缩图，默认二者都有
  sourceType: ['album', 'camera'], // 可以指定来源是相册还是相机，默认二者都有
  success: function (res) {
    // 返回选定照片的本地文件路径列表，tempFilePath可以作为img标签的src属性显示图片
    var tempFilePaths = res.tempFilePaths
  }
})
wx.previewImage({
  current: '', // 当前显示图片的http链接
  urls: [] // 需要预览的图片http链接列表
})
wx.getImageInfo({
  src: 'images/a.jpg',
  success: function (res) {
    console.log(res.width)
    console.log(res.height)
  }
})

wx.chooseImage({
  success: function (res) {
    wx.getImageInfo({
      src: res.tempFilePaths[0],
      success: function (res) {
        console.log(res.width)
        console.log(res.height)
      }
    })
  }
})
wx.saveImageToPhotosAlbum({
    success(res) {
    }
})
```
五、录音		
1.推荐API - v1.6.0+

	wx.getRecorderManager() 获取全局唯一的录音管理器 recorderManager

recorderManager对象方法列表

|方法|参数|描述|
|---|---|---|
|start|options|开始录音|
|pause||暂停录音|
|resume||继续录音|
|stop||停止录音|
|onStart| callback |录音开始事件|
|onPause| callback |录音暂停事件|
|onStop| callback |录音停止事件,会回调文件地址|
|onFrameRecorded| callback |已录制完指定帧大小的文件，会回调录音分片结果数据，如果设置了frameSize，则会回调此事件|
|onError|callback|录音错误事件，会回调错误信息|
	
2.不推荐API - v1.6.0-

	wx.startRecord  
		需要用户授权scope.record;
		用户离开小程序无法调用此API;
				
	wx.stopRecord
		自动调用结束或者录音超过1分钟，自动结束录音;
		返回录音文件的临时文件路径;
	文件的临时路径，在小程序本次启动期间可以正常使用，如需持久保存，需在主动调用 wx.saveFile，在小程序下次启动时才能访问得到;
3.eg		

```js
v1.6.0+方法
contact.wxml
 
 <button type='default' bindtap='startRecord'>开始录音</button>
 <button type='default' bindtap='stopRecord'>结束录音</button>
  
contact.js

const recorderManager = wx.getRecorderManager()
const options = {
  duration: 10000,
  sampleRate: 44100,
  numberOfChannels: 1,
  encodeBitRate: 192000,
  format: 'aac',
  frameSize: 50
}

Page（{
	
  startRecord: function(){
    
    recorderManager.start(options);
  },
  stopRecord: function(){

    recorderManager.stop();
  },
}）
recorderManager.onStart(() => {
  console.log('recorder start')
})
recorderManager.onPause(() => {
  console.log('recorder pause')
})
recorderManager.onStop((res) => {
  console.log('recorder stop', res)
  const { tempFilePath } = res
})
recorderManager.onFrameRecorded((res) => {
  const { frameBuffer } = res
  console.log('frameBuffer.byteLength', frameBuffer.byteLength)
})

v1.6.0-方法：
wx.startRecord({
  success: function(res) {
    var tempFilePath = res.tempFilePath 
  },
  fail: function(res) {
     //录音失败
  }
})
setTimeout(function() {
  //结束录音  
  wx.stopRecord()
}, 10000)
```
六、音频播放控制		
1.推荐API - v1.6.0+ 		

	wx.createInnerAudioContext() 创建并返回内部 audio 上下文 innerAudioContext 对象
	
2.不维护API - v1.6.0-	
	
	wx.playVoice() 开始播放语音	
		同时允许一个语音文件正在播放，如果前一个语音文件未播放完，将中断前一个语音播放;
	wx.pauseVoice() 暂停播放语音
		再次调用wx.playVoice播放同一个文件时，会从暂停处开始播放；
		如果想从头开始播放，需先调用wx.stopVoice();
	wx.stopVoice() 结束播放语音
七、音乐播放控制(背景音频播放管理)	
1.推荐API - v1.2.0+

	wx.getBackgroundAudioManager() 获取全局唯一的背景音频管理器 backgroundAudioManager
	backgroundAudioManager 对象属性列表
		duration 当前音频的长度（单位：s），只有在当前有合法的 src 时返回
		currentTime 当前音频的播放位置（单位：s），只有在当前有合法的 src 时返回
		paused
		src 音频的数据源，默认为空字符串，当设置了新的 src 时，会自动开始播放 ，目前支持的格式有 m4a, aac, mp3, wav
		startTime
		buffered 音频缓冲的时间点，仅保证当前播放时间点到此时间点内容已缓冲
		title
		epname
		singer
		coverImgeUrl 封面图url，用于做原生音频播放器背景图。原生音频播放器中的分享功能，分享出去的卡片配图及背景也将使用该图
		webUrl 页面链接，原生音频播放器中的分享功能，分享出去的卡片简介，也将使用该值
		protocol 音频协议。默认值为 'http'，设置 'hls' 可以支持播放 HLS 协议的直播音频
	对象方法列表：
		play
		pause
		stop
		seek
		onCanplay 背景音频进入可以播放状态，但不保证后面可以流畅播放
		onPause
		onStop
		onEnded
		onTimeUpdate 背景音频播放进度更新事件
		onPrev 用户在系统音乐播放面板点击上一曲事件（iOS only）
		onNext 用户在系统音乐播放面板点击下一曲事件（iOS only）
		onError
		onWaiting 音频加载中事件，当音频因为数据不足，需要停下来加载时会触发
	错误码
		10001		系统错误
		10002		网络错误
		10003		文件错误
		10004		格式错误
		-1			未知错误
	
2.不维护API - v1.2.0-
	
	wx.getBackgroundAudioPlayerState() 获取后台音乐播放状态
	wx.playBackgroundAudio() 使用后台播放器播放音乐
		对于微信客户端来说，只能同时有一个后台音乐在播放。
		当用户离开小程序后，音乐将暂停播放；
		当用户点击“显示在聊天顶部”时，音乐不会暂停播放；
		当用户在其他小程序占用了音乐播放器，原有小程序内的音乐将停止播放.
	wx.pauseBackgroundAudio() 暂停播放音乐
	wx.seekBackgroundAudio() 控制音乐播放进度
		iOS 6.3.30  会有短暂延迟
	wx.stopBackgroundAudio() 停止播放音乐
	wx.onBackgroundAudioPlay(CALLBACK) 监听音乐播放。
	wx.onBackgroundAudioPause(CALLBACK) 监听音乐暂停。
	wx.onBackgroundAudioStop(CALLBACK) 监听音乐停止。

八、实时音视频播放		

wx.createLivePlayerContext(domId, this)
	
	操作对应的 <live-player/> 组件。 创建并返回 live-player 上下文 LivePlayerContext 对象。在自定义组件下，第二个参数传入组件实例this，以操作组件内 <live-player/> 组件
	livePlayerContext 对象的方法列表
		play 播放
		stop 停止
		mute 静音
		pause	暂停
		resume		恢复
		requestFullScreen 进入全屏
		exitFullSrceen	退出全屏
	
wx.createLivePusherContext()
	
	创建并返回 live-pusher 上下文 LivePusherContext 对象，LivePusherContext 与页面的 <live-pusher /> 组件绑定，一个页面只能有一个 live-pusher，通过它可以操作对应的 <live-pusher/> 组件。 在自定义组件下，第一个参数传入组件实例this，以操作组件内 <live-pusher/> 组件
	livePusherContext 对象的方法列表
		start 开始推流
		stop 停止推流
		pause 暂停推流
		resume 恢复推流
		switchCamera 切换前后摄像头
		snapshot 快照


参考资料：		
1.[Audio](https://developers.weixin.qq.com/miniprogram/dev/component/audio.html)    
2.[音频组件API](https://developers.weixin.qq.com/miniprogram/dev/api/api-audio.html)    
3.[音频进度条](https://blog.csdn.net/Wu_shuxuan/article/details/78285875?locationNum=5&fps=1)  
4.[image组件](https://developers.weixin.qq.com/miniprogram/dev/component/image.html) 	
5.[imageAPI](https://developers.weixin.qq.com/miniprogram/dev/api/media-picture.html)		
6.[Camera组件和API](https://developers.weixin.qq.com/miniprogram/dev/component/camera.html)		
7.[录音](https://developers.weixin.qq.com/miniprogram/dev/api/media-record.html)		
8.[语音播放控制](https://developers.weixin.qq.com/miniprogram/dev/api/media-voice.html)	
9.[实时音视频](https://developers.weixin.qq.com/miniprogram/dev/api/api-live-player.html)	
  