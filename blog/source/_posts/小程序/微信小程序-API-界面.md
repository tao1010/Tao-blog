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

三、导航			
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
四、tabBar		
1.wx.setTabBarBadge - 为tabBar某一项的右上角添加文本    

	object参数说明：
		index tabBar的哪一项从左边算起 必填 Number
		text 显示的文本，超过3个字符则显示成'...' 必填 String
		success/fail/complete相关回调函数
	eg:
	wx.setTabBarBadge({
      index: 3,
      text: '100',
    })
2.wx.removeTabBarBadge - 移除tabBar某一项右上角的文本

	object参数说明：
		index tabBar的哪一项从左边算起 必填 Number
		success/fail/complete相关回调函数
	eg:
	setTimeout(function(){

      wx.removeTabBarBadge({
        index: 3,
      })
    },2000)
3.wx.showTabBarRedDot - 显示tabBar某一项的右上角的红点

	object参数说明：
		index tabBar的哪一项从左边算起 必填 Number
		success/fail/complete相关回调函数
	eg:
	wx.showTabBarRedDot({
      index: 3,
    })
4.wx.hideTabBarRedDot - 隐藏tabBar某一项的右上角的红点
	
	object参数说明：
		index tabBar的哪一项从左边算起 必填 Number
		success/fail/complete相关回调函数
	eg:
	setTimeout(function(){

      wx.hideTabBarRedDot({
        index: 3,
      })
    },2000)
5.wx.setTabBarStyle - 动态设置tabBar的整体样式
	
	object参数说明：
		color tab上的文字默认颜色 hexcolor
		selectedColor tab上的文字选中时的颜色 hexcolor
		backgroundColor tab的背景颜色
		borderStyle tabbar上边框的颜色，仅支持black/white
		success/fail/complete 相关回调函数
	eg:
	wx.setTabBarStyle({
      color: "#ff0000",
      selectedColor: '#00ff00',
      backgroundColor: "#0000ff",
      borderStyle: 'white'
    })

6.wx.setTabBarItem - 动态设置tabBar某一项的内容
	
	object参数说明：
		index tabBar的哪一项，从左边算起 必填 Number
		text tab上按钮文字 String
		iconPath 图片路径，icon 大小限制为40kb，建议尺寸为 81px * 81px，当 postion 为 top 时，此参数无效，不支持网络图片 String
		selectedIconPath 选中时的图片路径，icon 大小限制为40kb，建议尺寸为 81px * 81px ，当 postion 为 top 时，此参数无效
		success/fail/complete 相关回调函数
	eg:
	 wx.setTabBarItem({
        index: 3,
        text: '中心',
        iconPath: 'pages/images/nav/home.png',
        selectedIconPath: 'pages/images/nav/home-sel.png'
      })
7.wx.showTabBar - 显示tabBar

	object参数说明：
		animation 是否需要动画效果，默认无 boolean
		success/fail/complete 相关回调函数
	eg:
	 setTimeout(function(){
      wx.showTabBar({
        aniamtion: true
      })
    },2000)
8.wx.hideTabBar - 隐藏tabBar

	object参数说明：
		animation 是否需要动画效果，默认无 boolean
		success/fail/complete 相关回调函数
	eg:
	 wx.hideTabBar({
      animation: true
    })
      
五、动画		
1.wx.creatAnimation - 创建一个动画实例animtion    

	调用实例的方法来描述动画。最后通过动画实例的export方法导出动画数据传递给组件的animation属性；
	export 方法每次调用后会清掉之前的动画操作；
	
	object参数说明：
		
		duration 动画持续时间，单位ms 默认400 Integer
		timingFunction 定义动画的效果	默认‘linear’ String
		delay 动画延迟时间，单位ms 默认 0 Integer
		transformOrigin 设置transform-origin 默认‘50% 50% 0’ String
	timingFunction有效值：
		linear 动画从头到尾的速度是相同的
		ease	动画以低速开始，然后加快，在结束前变慢
		ease-in 动画以低速开始
		ease-in-out 动画以低速开始和结束
		ease-out 动画以低速结束
		step-start 动画第一帧就跳至结束状态到结束
		step-end 动画一直保持开始状态，最后一帧跳到结束状态
2.animation - 动画实例
	
	可以调用以下方法来描述动画，调用结束后会返回自身，支持链式调用的写法；
	animation对象的方法列表：
	   样式：
			opacity 透明度，参考范围0-1 value
			backgroundColor 颜色值 color
			width 长度值，如果传入 Number 则默认使用 px，可传入其他自定义单位的长度值 length
			height ...
			top ...
			left ...
			bottom ...
			right ...
		旋转
			rotate  deg deg的参数范围-180～180，从原点顺时针旋转一个deg角度
			rotateX deg deg的参数范围-180～180，在X轴旋转一个deg角度
			rotateY deg deg的参数范围-180～180，在Y轴旋转一个deg角度
			rotateZ deg deg的参数范围-180～180，在Z轴旋转一个deg角度
			rotate3d  (x,y,z,deg)
		缩放
			scale  sx,[sy] 一个参数时，表示在X轴、Y轴同时缩放sx倍数；两个参数时表示在X轴缩放sx倍数，在Y轴缩放sy倍数
			scaleX sx 在X轴缩放sx倍数
			scaleY sy 在Y轴缩放sy倍数
			scaleZ sz 在Z轴缩放sz倍数
			scale3d(sx,sy,sz) 在X轴缩放sx倍数，在Y轴缩放sy倍数，在Z轴缩放sz倍数
		偏移
			translate	 tx,[ty] 一个参数时，表示在X轴偏移tx，单位px；两个参数时，表示在X轴偏移tx，在Y轴偏移ty，单位px。
			translateX tx 在X轴偏移tx，单位px
			translateY ty 在Y轴偏移ty，单位px
			translateZ tz 在Z轴偏移tz，单位px
			translate3d (tx,ty,tz) 在X轴偏移tx，在Y轴偏移ty，在Z轴偏移tz，单位px
		倾斜
			skew ax,[ay] 参数范围-180~180；一个参数时，Y轴坐标不变，X轴坐标延顺时针倾斜ax度；两个参数时，分别在X轴倾斜ax度，在Y轴倾斜ay度
			skewX ax 参数范围-180~180；Y轴坐标不变，X轴坐标延顺时针倾斜ax度
			skewY ay 参数范围-180~180；X轴坐标不变，Y轴坐标延顺时针倾斜ay度
		矩阵变形
			matrix (a,b,c,d,tx,ty)
			matri3d
	
3.动画队列
	
	调用动画操作方法后要调用 step() 来表示一组动画完成，可以在一组动画中调用任意多个动画方法，一组动画中的所有动画会同时开始，一组动画完成后才会进行下一组动画；
	step 可以传入一个跟 wx.createAnimation() 一样的配置参数用于指定当前组动画的配置
4.bug:iOS/Android 6.3.30 通过 step() 分隔动画，只有第一步动画能生效     
5.eg：      

    onReady: function(){
	    //创建实例对象animation
	    this.animation = wx.createAnimation()
  	 },
	 //旋转
    rotate:function(){
	    
	    //从原点顺时针旋转一个deg角度
	    // this.animation.rotate(Math.random() * 720 - 360).step()
	    //在X轴旋转一个deg角度
	    // this.animation.rotateX(Math.random() * 720 - 360).step()
	    //在Y轴旋转一个deg角度
	    // this.animation.rotateY(Math.random() * 720 - 360).step()
	    //在Z轴旋转一个deg角度  
	    // this.animation.rotateZ(Math.random() * 720 - 360).step()
	    //在3d轴旋转一个deg角度
	    this.animation.rotate3d(10,10,10,Math.random() * 720 - 360).step()
	    this.setData({
	      animation: this.animation.export()
	    })
	  },
	//缩放
	scale:function(){
	
	    //一个参数表示在x，y轴同时缩放sx倍
	    // this.animation.scale(Math.random()*2).step()
	    //两个参数时表示在X轴缩放sx倍数，在Y轴缩放sy倍数
	    // this.animation.scale(Math.random() * 2, [Math.random() * 3]).step()
	    //在X轴缩放
	    // this.animation.scaleX(Math.random()*2).step()
	    //在Y轴缩放
	    // this.animation.scaleY(Math.random() * 2).step()
	    //在Z轴缩放
	    // this.animation.scaleZ(Math.random() * 2).step()
	    //3d缩放(sx,sy,sz)
	    this.animation.scale3d(Math.random() * 1, Math.random() * 2, Math.random() * 3).step()
	    this.setData({
	      animation: this.animation.export()
	    })
	  },
    //偏移
	translate:function(){
	    //一个参数表示在X，Y轴同时偏移tx
	    // this.animation.translate(Math.random() * 100 - 50).step()
	    //两个参数表示在X，Y轴分别偏移tx，ty
	    // this.animation.translate(Math.random() * 100 - 50, Math.random() * 100 - 80).step()
	    //在X轴移动
	    // this.animation.translateX(Math.random() * 100 - 50).step()
	    //在Y轴移动
	    // this.animation.translateY(Math.random() * 100 - 50).step()
	    //在Z轴移动
	    // this.animation.translateZ(Math.random() * 100 - 50).step()
	    //在3d移动
	    this.animation.translate3d(Math.random() * 100 - 50, Math.random() * 100 - 60, Math.random() * 100 - 70).step()
	    this.setData({
	      animation: this.animation.export()
	    })
	  },
	 //倾斜
    skew:function(){
    
	    //一个参数，Y轴不变，X轴在沿顺时针方向倾斜ax度
	    // this.animation.skew(Math.random() * 90).step()
	    //两个参数，分别在X轴倾斜ax度，在Y轴倾斜ay度
	    // this.animation.skew(Math.random() * 90,Math.random() * 45).step()
	    //在X轴倾斜ax度
	    this.animation.skewX(Math.random() * 90).step()
	    //在Y轴倾斜ay度
	    // this.animation.skewY(Math.random() * 90).step()
	    this.setData({
	      animation: this.animation.export()
	    })
    },
    //矩阵变形
    ...
    
    //旋转并缩放
	rotateAndScale:function(){
	    this.animation.rotate(Math.random() * 720 - 360)
	                  .scale(Math.random() * 2)
	                  .step()
	    this.setData({
	      animation: this.animation.export()
	    })
	},
	//旋转后缩放
	rotateThenScale:function(){
	
	    this.animation.rotate(Math.random() * 720 - 360).step()
	      .scale(Math.random() * 2).step()
	    this.setData({
	      animation: this.animation.export()
	    })
	},
	
	//同时展示全部
	all:function(){
	    this.animation.rotate(Math.random() * 720 - 360)
	      .scale(Math.random() * 2)
	      .translate(Math.random() * 100 - 50, Math.random() * 100 - 50)
	      .skew(Math.random() * 90, Math.random() * 90)
	      .step()
	    this.setData({
	       animation: this.animation.export() 
	    })
	},
	//顺序展示全部
	allInQueue:function(){
	    this.animation.rotate(Math.random() * 720 - 360).step()
	      .scale(Math.random() * 2).step()
	      .translate(Math.random() * 100 - 50, Math.random() * 100 - 50).step()
	      .skew(Math.random() * 90, Math.random() * 90).step()
	    this.setData({
	       animation: this.animation.export() 
	    })
	},
六、位置		
wx.pageScrollTo - 将页面滚动到目标位置
	
	object参数说明:
		scrollTop 滚动到页面的目标位置，单位px
		duration 滚动动画的时长，单位ms
	eg:
	wx.pageScrollTo({
		scrollTop: 0,
		duration: 3000
	})
 
七、绘图
1.API		

	wx.createCanvasContext - 创建canvas绘画上下文(指定canvasId)
		在自定义组件中，第二个参数传入组件实例this，以操作组件内<canvas/>组件；如果省略第二个参数，则不在任何自定义组件内查找；
	wx.createContext - 创建canvas绘图上下文(不推荐使用)
	wx.drawCanvas - 进行绘图(不推荐使用)
		参数说明：
		canvasId 画布标识，传入<canvas/>的canvas-id String
		actions 绘图动作数组，由 wx.createContext 创建的 context，调用 getActions 方法导出绘图动作数组
		reserve 本次绘制是否接着上一次绘制，即reserve参数为false，则在本次调用drawCanvas绘制之前native层应先清空画布再继续绘制；若reserver参数为true，则保留当前画布上的内容，本次调用drawCanvas绘制的内容覆盖在上面，默认 false
		用所提供的actions在所给的canvas-id对应的canvas上进行绘图；
	wx.canvasToTempFilePath - 导出图片
		把当前画布指定区域的内容导出生成指定大小的图片，并返回文件路径。在自定义组件下，第二个参数传入组件实例this，以操作组件内 <canvas/> 组件
		在 draw 回调里调用该方法才能保证图片导出成功；
		object参数说明：
			x  			画布x轴起点(默认0) Number
			y  			画布y轴起点(默认0) Number
			width 		画布宽度（默认为canvas宽度-x） Number
			height 	画布高度（默认为canvas高度-y）Number
			destWidth 输出图片宽度（默认为 width * 屏幕像素密度） Number
			destHeight 输出图片高度（默认为 height * 屏幕像素密度）Number
			canvasId   画布标识，传入 <canvas/> 的 canvas-id
			fileType   目标文件的类型，只支持 'jpg' 或 'png'。默认为 'png'
			quality 	 图片的质量，取值范围为 (0, 1]，不在范围内时当作1.0处理
			success/fail/complete 回调函数
	wx.canvasGetImageData - 返回一个数组，用来描述canvas区域隐含的像素数据
	wx.canvasPutImageData - 将像素数据绘制到画布的方法
			
2.context对象的方法列表
	
	颜色、样式、阴影
		setFillStyle - 设置填充颜色 默认black
		setStrokeStyle - 设置边框颜色 默认black
		setShadow - 设置阴影样式
			 如果没有设置，offsetX 默认值为0， 设置阴影相对于形状在水平方向的偏移
			 				offsetY 默认值为0， 设置阴影相对于形状在竖直方向的偏移
			 				blur 默认值为0，设置阴影的模糊级别 0～100，数值越大越模糊
			 				color 默认值为 black；设置阴影的颜色
	渐变
		createLinearGradient - 创建一个线性渐变
		createCircularGradient - 创建一个圆形渐变 
		addColorStop - 在渐变中的某一点添加一个颜色渐变addColorStop(stop,color)
			小于最小 stop 的部分会按最小 stop 的 color 来渲染，大于最大 stop 的部分会按最大 stop 的 color 来渲染；
	线条样式
		setLineWidth - 设置线条宽度
		setLineCap - 设置线条端点样式
			lineCap参数说明:'butt'、'round'、'square'
		setLineJoin - 设置两线相交处的样式
			lineJoin参数说明:'bevel'、'round'、'miter'
		setLineDash - 设置虚线的线条宽度
			参数说明：
			pattern 一组描述交替绘制线段和间距长度的数字 Array
			offset 虚线偏移量 Number
			eg:setLineDash([实线长度, 虚线长度], 虚线偏移量)
		setMiterLimit - 设置最大斜接长度
			斜接长度指的是在两条线交汇处内角和外角之间的距离；
			当 setLineJoin() 为 miter 时才有效。超过最大倾斜长度的，连接处将以 lineJoin 为 bevel 来显示；
	矩形
		rect - 创建一个矩形
			 用 fill() 或者 stroke() 方法将矩形真正的画到 canvas 中
		fillRect - 填充一个矩形	
			用 setFillStyle() 设置矩形的填充色，如果没设置默认是黑色
		strokeRect - 画一个矩形（不填充）
			用 setFillStroke() 设置矩形线条的颜色，如果没设置默认是黑色
		clearRect - 在给定的矩形区域内，清除画布上的像素
			
	路径
		fill - 对当前路径进行填充，默认的填充色为黑色
			 如果当前路径没有闭合，fill() 方法会将起点和终点进行连接，然后填充；
			 fill() 填充的的路径是从 beginPath() 开始计算，但是不会将 fillRect() 包含进去；
		stroke - 对当前路径进行描边 默认颜色色为黑色
			stroke() 描绘的的路径是从 beginPath() 开始计算，但是不会将 strokeRect() 包含进去
		beginPath - 开始一个路径
			需要调用fill或者stroke才会使用路径进行填充或描边；
			同一个路径内的多次setFillStyle()、setStrokeStyle()、setLineWidth()等设置，以最后一次设置为准；
		closePath - 关闭一个路径
			如果关闭路径后没有调用 fill() 或者 stroke() 并开启了新的路径，那之前的路径将不会被渲染；
		moveTo - 把路径移动到画布中的指定点，但不创建线条
			用 stroke() 方法来画线条
		lineTo - 添加一个新点，然后在画布中创建从该点到最后指定点的线条
		arc - 添加一个弧形路径到当前路径，顺时针绘制
			创建一个圆可以用 arc() 方法指定其实弧度为0，终止弧度为 2 * Math.PI;
			用 stroke() 或者 fill() 方法来在 canvas 中画弧线;
			参数说明：
				x 圆心x
				y 圆心y
				r 半径
				sAngle 起始弧度，单位弧度（在3点钟方向）
				eAngle 终止弧度
				counterclockwise 指定弧度的方向是逆时针还是顺时针。默认是false，即顺时针
		quadraticCurveTo - 创建二次方贝塞尔曲线
			曲线的起始点为路径中前一个点；
			参数说明：
				cpx 贝塞尔控制点x坐标
				cpy 贝塞尔控制点y坐标
				x	结束点x
				y	结束点y
		bezierCurveTo - 创建三次方贝塞尔曲
			曲线的起始点为路径中前一个点；
			参数说明：
				cp1x 第一个贝塞尔控制点x坐标
				cp1y 第一个贝塞尔控制点y坐标
				cp2x 第二个贝塞尔控制点x坐标
				cp2y 第二个贝塞尔控制点y坐标
				x	结束点x
				y	结束点y
	变形
		scale - 对横纵坐标进行缩放
			在调用scale方法后，之后创建的路径其横纵坐标会被缩放。多次调用scale，倍数会相乘；
		rotate - 对坐标轴进行顺时针旋转
			以原点为中心，原点可以用 translate方法修改。顺时针旋转当前坐标轴。多次调用rotate，旋转的角度会叠加
		translate - 对坐标原点进行缩放
			对当前坐标系的原点(0, 0)进行变换，默认的坐标系原点为页面左上角
	文字
		clip - 从原始画布中剪切任意形状和尺寸
			一旦剪切了某个区域，则所有之后的绘图都会被限制在被剪切的区域内（不能访问画布上的其他区域）;
			可以在使用 clip() 方法前通过使用 save() 方法对当前画布区域进行保存，并在以后的任意时间对其进行恢复（通过 restore() 方法）;
		fillText - 在画布上绘制被填充的文本
			参数说明:
				text 在画布上输出的文本 String
				x		绘制文本的左上角x坐标位置
				y		绘制文本的左上角y坐标位置
				maxWidth	需要绘制的最大宽度，可选
		setFontSize - 设置字体大小
			参数说明： 
				fontSize 字体的字号 Number
		setTextBaseLine - 设置字体基准线
			参数说明：
				textBaseline 可选值  'top'、'bottom'、'middle'、'normal'
		setTextAlign - 设置字体对齐方式
			参数说明：
				align 可选值 ‘left’、‘center’、‘right’
	图片
		drawImage - 在画布上绘制图像
			参数说明：
				imageResource  所要绘制的图片资源
				dx	图像的左上角在目标canvas上 X 轴的位置
				dy
				dWidth	在目标画布上绘制图像的宽度，允许对绘制的图像进行缩放
				dHeight
				sx	源图像的矩形选择框的左上角 X 坐标
				sy
				sWidth	源图像的矩形选择框的高度
				sHeight
			三个版本写法：
				drawImage(dx, dy)
				drawImage(dx, dy, dWidth, dHeight)
				drawImage(sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight) 从 1.9.0 起支持
	混合
		setGlobalAlpha - 设置全局画笔透明度
	其他
		save - 保存当前绘图的上下文
		restore - 恢复之前保存过的绘图上下文
		draw - 进行绘图
			将之前在绘图上下文中的描述（路径、变形、样式）画到 canvas 中；
			绘图上下文需要由 wx.createCanvasContext(canvasId) 来创建；
			参数说明：
				reserve 非必填。本次绘制是否接着上一次绘制，即reserve参数为false，则在本次调用drawCanvas绘制之前native层应先清空画布再继续绘制；
						若reserver参数为true，则保留当前画布上的内容，本次调用drawCanvas绘制的内容覆盖在上面，默认 false
				callback 绘制完成后回调
		
		getActions - 获取当前context上存储的绘图动作(不推荐使用)
		clearActions - 清空当前的存储绘图动作(不推荐使用)
		measureText - 测量文本尺寸信息，目前仅返回文本宽度。同步接口
			参数：text 要测量的文本 String
			返回参数：返回TextMetrucs对象 width 文本宽度 Number		arcTo - 根据控制点和半径绘制圆弧路径
			canvasContext.arcTo(x1, y1, x2, y2, radius)
			第一控制点(x1,y1)第二控制点(x2,y2)
			radius 圆弧半径
		strokeText - 给定的 (x, y) 位置绘制文本描边的方法
			canvasContext.strokeText(text, x, y, maxWidth)
			文本起始点坐标(x,y)
		lineDashOffset - 设置虚线偏移量的属性 初始值为0
		createPattern - 对指定的图像创建模式的方法，可在指定的方向上重复元图像
			canvasContext.createPattern(image, repetition)
			image:重复的图像源，仅支持包内路径和临时路径
			repetition ：指定如何重复图像，有效值有: repeat, repeat-x, repeat-y, no-repeat
		font - 设置当前字体样式的属性
			style 字体样式。仅支持 italic, oblique, normal
			weight 字体粗细。仅支持 normal, bold
			size 字体大小
			family 字体族名。注意确认各平台所支持的字体
		setTransform - 使用矩阵重新设置（覆盖）当前变换的方法
			canvasContext.setTransform(scaleX, skewX, skewY, scaleY, translateX, translateY)
			scaleX 水平缩放
			skewX 水平倾斜
			skewY 垂直倾斜
			scaleY 垂直缩放
			translateX 水平移动
			translateY 垂直移动
3.在Canvas上画图		
	
	所有在<canvas/>中的画图必须用JS完成；
	在JS中一般在onLoad中；
	
	步骤：
	1.创建一个Canvas绘图上下文CanvasContext；
		const ctx = wx.createCanvasContext('myCanvas', this)
	2.使用Canvas绘图上下文CanvasContext 进行绘图描述；
		ctx.setFillStyle('red')//设置填充颜色
       ctx.fillRect(10,10,150,75)//填充一个矩形fillRect(x, y, width, height)
	3.绘图
		ctx.draw()//进行绘图	
4.Canvas坐标系
	
	canvas是在一个二维的网格当中；
	左上角的左标为(0,0);
5.渐变
	
	能用于填充一个矩形、圆、线、文字等；
	
	颜色渐变的方式：
		const grd = ctx.createLinearGradient(x,y,x1,y1)//创建一个线性的渐变
	一旦创建了一个渐变对象，必须添加两个指定颜色的渐变点：
		grd.addColorStop(postion,color) // 用于指定颜色渐变点的位置和颜色，postion必须位于0到1之间
    	grd.addColorStop(postion,color)
    可以用setFillStyle() 和 setStrokeStyle() 方法设置渐变，然后进行画图描述；
	
	eg:
	
	const ctx = wx.createCanvasContext('myCanvas', this)
	
	//线性渐变
    const grd = ctx.createLinearGradient(0,0,200,0)
    
	//圆心渐变
	const grd = ctx.createCircularGradient(75,50,50)

	grd.addColorStop(0,'red')
    grd.addColorStop(1,'white')
    ctx.setFillStyle(grd)
	ctx.fillRect(10,10,150,75)//填充一个矩形
    ctx.draw()//进行绘图
6.color
	
	表示canvas中使用的颜色：
		RGB    	'rgb(255,0,0)'
		RGBA	 	'rgba(255,0,0,0.3)'
		16进制 	  	'#ff0000'
		预定义颜色  'red'
	color name 大小写不敏感


八、下拉刷新
1.Page.onPullDownRefresh - Page中配置下拉刷新
	
	在Page中定义onPullDownRefresh 处理函数,监听该页面用户下拉刷新事件；
		需要在config中的window选项中开启 enablePullDownRefresh;
		"window": {
			...
		    "enablePullDownRefresh": true
	  	},
2.wx.startPullDownRefresh - 开始下拉刷新
	
	调用后触发下拉刷新动画，效果与用户手动下拉刷新一致；
	object参数说明:
		success/fail/complete相关回调函数
	success参数说明：
		errMsg 接口调用结束 String
3.wx.stopPullDownRefresh - 停止当前页面的下拉刷新
	
	当处理完数据刷新后，停止下拉刷新；
九、WXML节点 
	...





参考资料：		
1.[API界面](https://developers.weixin.qq.com/miniprogram/dev/api/api-react.html)		
2.[tabBar](https://developers.weixin.qq.com/miniprogram/dev/api/ui-tabbar.html)		
3.[transform-function](https://developer.mozilla.org/en-US/docs/Web/CSS/transform-function/matrix)        
4.[绘图](https://developers.weixin.qq.com/miniprogram/dev/api/canvas/reference.html)