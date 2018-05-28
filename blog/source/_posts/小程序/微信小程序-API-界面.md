---
title: 微信小程序-API-界面
date: 2018-05-28 09:02:22
tags: 微信小程序
categories: 小程序
---

一、交互反馈		
1.wx.showToast - 消息提示框 		
	
	object参数说明：
		title  提示的内容 必填 String
		icon 图标(有效值'success'/'loading'/'none') String
			success和loading时，title最大显示7个汉字长度；
			none时，title最多显示2行；
		image 自定义图标的本地路径(优先级高于icon) String
		duration 提示延长的时间，单位毫秒 默认1500ms
		mask 是否显示透明蒙层，防止触摸穿透 默认false
		success/fail/complete相关回调函数
	
	wx.hideToast - 隐藏消息提示框
	
	eg：
	 wx.showToast({
      title: '登录中...',
      icon: 'loading',
      duration: 20000,
      success: function(res){
        console.log('成功回调:' + res.errMsg)
        wx.hideToast()
      }
    })
    setTimeout(function(){
      wx.hideToast()
    }, 2000)
    
2.wx.showLoading - 显示loading提示框
	
	需要主动调用wx.hideLoading才能关闭提示框；
	
	object参数说明:
		title  提示的内容 必填 String
		mask 是否显示透明蒙层，防止触摸穿透 默认false
		success/fail/complete相关回调函数
	
	wx.hideLoading - 隐藏提示框
		
	eg：
	 wx.showLoading({
      title: '登录中...',
      // mask: true
    })
    setTimeout(function(){
      wx.hideLoading()
    }, 2000)	
3.wx.showModal - 显示模态弹框

	object参数说明：
		title 提示的标题 必填 String
		content 提示的内容 必填 String
		showCancel 是否显示取消按钮 默认true
		cancelText 取消按钮的文字 默认‘取消’ 最多4个字符 String
		cancelColor 取消按钮的字体颜色 默认‘#000000’ HexColor
		confirmText 确定按钮的文字 默认'确定' 最多4个字符 String
		confirmColor 确定按钮的字体颜色 默认‘#3cc51f’ HexColor
		success/fail/complete相关回调函数
	
	success返回参数说明：
		confirm 为true时，表示点击了确定按钮
		cancel 为true时，表示点击了取消按钮
	
	eg:
	wx.showModal({
      title: '登录',
      content: '这是一个模态弹框',
      success: function(res){

        if(res.confirm){
          console.log('点击了确定按钮')
        }else if(res.cancel){
          console.log('点击了取消按钮')
        }
      }
    })
4.wx.shoaActionSheet - 显示操作菜单		

	object参数说明：
		itemList 按钮的文字数组，数组长度最大为6个 必填 String Array
		itemColor 按钮的文字颜色 默认‘#000000’ HexColor
		success/fail/complete相关回调函数
	eg:
	wx.showActionSheet({
      itemList: ['教师','家长','学校','管理员'],
      itemColor:'#3CC51F',
      success: function (res) {
        console.log(res.tapIndex)
      },
      fail: function (res) {
        console.log(res.errMsg)
      }
    })		
5.tips：
	
	bug: Android 6.3.30，wx.showModal 的返回的 confirm 一直为 true；
	tip: wx.showActionSheet 点击取消或蒙层时，回调 fail, errMsg 为 "showActionSheet:fail cancel"；
	tip: wx.showLoading 和 wx.showToast 同时只能显示一个，但 wx.hideToast/wx.hideLoading 也应当配对使用；
	tip: iOS wx.showModal 点击蒙层不会关闭模态弹窗，所以尽量避免使用“取消”分支中实现业务逻辑。
二、导航条		
<font color='red'>1.wx.setTopBarText - 动态设置顶栏文字内容</font> 	   

	只有当前小程序被置顶时能生效，如果当前小程序没有被置顶，也能调用成功，但是不会立即生效，只有在用户将这个小程序置顶后才换上设置的文字内容。
	调用成功后，需间隔 5s 才能再次调用此接口，如果在 5s 内再次调用此接口，会回调 fail，errMsg："setTopBarText: fail invoke too frequently"
	object参数说明：
		text 页面标题 必填 String
		success/fail/complete相关回调函数	
	eg：
	wx.setTopBarText({
      text: '设置',
      success: function(res){
        console.log(res)
      },
      fail: function (res) {
        console.log(res)
      }
    })
2.wx.setNavigationBarTitle - 动态设置当前页面的标题

	object参数说明：
		title 页面标题 必填 String
		success/fail/complete相关回调函数	
	eg：
	wx.setNavigationBarTitle({
      title: '更换导航栏标题',
      success: function(res){
        console.log(res)
      },
      fail:function(res){
        console.log(res)
      }
    })
3.wx.showNavigationBarLoading - 在当前页面显示导航条加载动画
	
	wx.showNavigationBarLoading();
	
4.wx.hideNavigationBarLoading - 隐藏导航条加载动画
	
    setTimeout(function(){

      wx.hideNavigationBarLoading()
    }, 2000)

三、tabBar			
1.wx.navigateTo - 跳转到应用内某个页面,保留当前页面
	
	使用wx.navigateBack 可以返回到原页面；
	object参数说明：
		url 需要跳转的应用内非 tabBar 的页面的路径 , 路径后可以带参数。参数与路径之间使用?分隔，参数键与参数值用=相连，不同参数用&分隔；如 'path?key=value&key2=value2' 必填 String
		success/fail/complete相关回调函数
	eg:
	wx.navigateTo({
      url: '../video/API/videoAPI',
    })
	tips:
		目前页面路径最多只能10层；
2.wx.redirectTo - 跳转到应用内某个页面,关闭当前页面
	
	object参数说明：
		url 需要跳转的应用内非 tabBar 的页面的路径 , 路径后可以带参数。参数与路径之间使用?分隔，参数键与参数值用=相连，不同参数用&分隔；如 'path?key=value&key2=value2' 必填 String
		success/fail/complete相关回调函数
	eg:
	wx.redirectTo({
      url: '../../common/index/index',
    })
3.wx.reLaunch - 打开到应用内的某个页面，关闭所有页面
	
	object参数说明：
		url 需要跳转的应用内非 tabBar 的页面的路径 , 路径后可以带参数。参数与路径之间使用?分隔，参数键与参数值用=相连，不同参数用&分隔；如 'path?key=value&key2=value2' 必填 String
		success/fail/complete相关回调函数
	eg:
	wx.reLaunch({
      url: '../../parents/my/my',
    })
4.wx.swtichTab - 跳转到tabBar页面，关闭其他所有非tabBar页面
	
	object参数说明：
		url 需要跳转的 tabBar 页面的路径（需在 app.json 的 tabBar 字段定义的页面），路径后不能带参数 必填 String
		success/fail/complete相关回调函数
	eg:
	wx.switchTab({
      url: '../../parents/my/my',
    })
5.wx.navigateBack - 关闭当前页面，返回上一页面或多级页面

	可通过 getCurrentPages()) 获取当前的页面栈，决定需要返回几层
	object参数说明：
		delta 返回的页面数，如果 delta 大于现有页面数，则返回到首页，默认1 Number
	eg:
	// 注意：调用 navigateTo 跳转时，调用该方法的页面会被加入堆栈，而 redirectTo 方法则不会。
	// 此处是A页面
	wx.navigateTo({
	  url: 'B?id=1'
	})
	
	// 此处是B页面
	wx.navigateTo({
	  url: 'C?id=1'
	})
	
	// 在C页面内 navigateBack，将返回A页面
	wx.navigateBack({
	  delta: 2
	})
6.tips
	
	 wx.navigateTo 和 wx.redirectTo 不允许跳转到 tabbar 页面，只能用 wx.switchTab 跳转到 tabbar 页面	
四、导航


五、动画


六、位置
 
 
七、绘图


八、下拉刷新



九、WXML节点 






参考资料：		
1.[API界面](https://developers.weixin.qq.com/miniprogram/dev/api/api-react.html)		
2.[tabBar](https://developers.weixin.qq.com/miniprogram/dev/api/ui-tabbar.html)		
	
