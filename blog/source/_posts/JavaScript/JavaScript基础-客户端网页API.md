---
title: JavaScript基础-客户端网页API
date: 2018-04-23 08:15:34
tags: JavaScript
categories: Web
---

一、Web API简介		

<!--more-->

1.概念:API(application interface)应用程序接口是以编程语言提供的结构，允许开发人员更容易地创建复杂的功能.它们抽象出更简单的代码，并提供一些简单的语法类使用.	
2.客户端JavaScript中的API并不是JavaScript语言的一部分，建立在JavaScript语言核心的顶部，分两类:

	浏览器API:内置于Web浏览器中，能从浏览器和电脑周边环境中提取数据，并用来做有用的复杂的事情.
	第三方API:缺省情况下不会内置于浏览器中，通常必须在Web中的某个地方获取代码和信息.
二、JavaScript、API、其他JavaScript工具 三者间的关系    

JavaScript
	
	一种内置于浏览器的高级脚本语言，您可以用来实现Web页面/应用中的功能。注意JavaScript也可用于其他象Node这样的的编程环境。
API
		
	客户端API — 内置于浏览器的结构程序，位于JavaScript语言顶部，可以更容易的实现功能。
	第三方API — 置于第三方普通的结构程序（例如Twitter，Facebook），使您可以在自己的Web页面中使用那些平台的某些功能。
其他JavaScript工具

	JavaScript库 — 通常是包含具有特定功能的一个或多个JavaScript文件，把这些文件关联到您的Web页以快速或授权编写常见的功能。例如包含jQuery和Mootools
	JavaScript框架 — 从库开始的下一步，JavaScript框架视图把HTML、CSS、JavaScript和其他安装的技术打包在一起，然后用来从头编写一个完整的Web应用。
三、API作用		
1.常见浏览器API	
	
操作文档的API

	eg:DOM API允许操作HTML和CSS编辑HTML，动态更新页面;
从服务器获取数据API

	eg:修改页面的某个细节数据。API包括XMLHttpRequest和Fetch API（术语Ajax）;
用于绘制和操作图形的API	

	允许您以编程方式更新包含在HTML <canvas> 元素中的像素数据以创建2D和3D场景的Canvas和WebGL；
	
	eg:绘制矩形或圆形等形状，将图像导入到画布上，然后使用Canvas API对其应用滤镜（如棕褐色滤镜或灰度滤镜），或使用WebGL创建具有光照和纹理的复杂3D场景;
音频和视频API

	eg：HTMLMediaElement，Web Audio API和WebRTC允许您使用多媒体来做一些事情，比如创建用于播放音频和视频的自定义UI控件，显示字幕字幕和您的视频，从网络摄像机抓取视频，通过画布操纵或在网络会议中显示在别人的电脑上，或者添加效果到音轨（如增益，失真，平移等）;
设备API

	以对网络应用程序有用的方式操作和检索现代设备硬件中的数据的API;
	eg:获取地理位置，系统通知，震动硬件.
客户端存储API

	创建一个应用程序来保存页面加载之间的状态，甚至让设备在处于脱机状态时可用;
	eg：使用Web Storage API的简单的键 - 值存储以及使用IndexedDB API的更复杂的表格数据存储;
		
2.常见第三方API
	
Twitter API

	在网站上展示你的Twitter等；
Google Maps API

	在网页上对地图进行操作等；
Facebook suite of API

	允许将facebook的生态系统中的功能用的应用中(登录、支付、推送等)；
YouTube API

	嵌入YouTube视频到网页中等；
Twilio API 

	针对语音通话和视频聊天的框架，发送短信或多媒体等；
四、API工作原理	
	
1.它们基于对象的	
eg:Geolocation API组成：
	
	Geolocation, 其中包含三种控制地理数据检索的方法
	Position, 表示在给定的时间的相关设备的位置。 — 它包含一个当前位置的 Coordinates 对象。还包含了一个时间戳,这个时间戳表示获取到位置的时间。
	Coordinates, 其中包含有关设备位置的大量有用数据，包括经纬度，高度，运动速度和运动方向等。

```html
<head>
    <meta charset="utf-8">
    <title>Google Maps example</title>

    <script type="text/javascript" src="https://maps.google.com/maps/api/js?key=AIzaSyDDuGt0E5IEGkcE6ZfrKfUtE9Ko_de66pA"></script>

    <style>
      #map_canvas {
        width: 600px;
        height: 600px;
      }
    </style>
</head>
<body>
    <h1>Simple maps example</h1>

    <div id="map_canvas"></div>
</body>
```
```js
//获取设备的当前位置,当设备的当前位置被成功取到时，这个函数会运行
navigator.geolocation.getCurrentPosition(function(position) {

//使用google.maps.LatLng()构造函数创建一个LatLng对象实例， 该构造函数需要我们的地理定位 Coordinates.latitude 和 Coordinates.longitude值作为参数：
  var latlng = new google.maps.LatLng(position.coords.latitude,position.coords.longitude);
  
  var myOptions = {
    zoom: 8,
    center: latlng,
    mapTypeId: google.maps.MapTypeId.TERRAIN,
    disableDefaultUI: true
  }
  var map = new google.maps.Map(document.querySelector("#map_canvas"), myOptions);
});

解析:
	首先，API对象通常包含构造函数，可以调用这些构造函数来创建用于编写程序的对象的实例。 
	其次，API对象通常有几个可用的options(如上面的myOptions对象)，可以调整以获得您的程序所需的确切环境(根据不同的环境,编写不同的Options对象)。 
	API构造函数通常接受options对象作为参数，这是您设置这些options的地方。
```
2.它们有可识别的入口点		
使用API时，应确保知道API入口点的位置.		

文档对象模型 (DOM) API有一个更简单的入口点 —它的功能往往被发现挂在 Document 对象, 或任何你想影响的HTML元素的实例.

	var em = document.createElement('em'); // create a new em element
	var para = document.querySelector('p'); // reference an existing p element
	em.textContent = 'Hello there!'; // give em some text content
	para.appendChild(em); // embed em inside para
其他API具有稍微复杂的入口点，通常涉及为要编写的API代码创建特定的上下文.
	
	Canvas API的上下文对象是通过获取要绘制的 <canvas> 元素的引用来创建的，然后调用它的HTMLCanvasElement.getContext()方法：
	var canvas = document.querySelector('canvas');
	var ctx = canvas.getContext('2d');
	
	然后，我们想通过调用内容对象 (它是CanvasRenderingContext2D的一个实例)的属性和方法来实现我们想要对画布进行的任何操作：
	
	Ball.prototype.draw = function() {
	  ctx.beginPath();
	  ctx.fillStyle = this.color;
	  ctx.arc(this.x, this.y, this.size, 0, 2 * Math.PI);
	  ctx.fill();
	};
3.它们使用事件来处理状态的变化
XMLHttpRequest 对象的实例  （每一个实例都代表一个到服务器的HTTP请求,来取得某种新的资源）都有很多事件可用，例如  onload 事件在成功返回时就触发包含请求的资源，并且现在就可用：
	
	var requestURL = 'https://mdn.github.io/learning-area/javascript/oojs/json/superheroes.json';
	var request = new XMLHttpRequest();
	request.open('GET', requestURL);
	request.responseType = 'json';
	request.send();
	
	//处理响应事件
	request.onload = function() {
	  var superHeroes = request.response;
	  populateHeader(superHeroes);
	  showHeroes(superHeroes);
	}	
4.它们在适当的地方有额外的安全机制		
WebAPI功能受到与JavaScript和其他Web技术（例如同源政策）相同的安全考虑 但是他们有时会有额外的安全机制(推送和位置弹框提醒是否允许访问)。

参考资料：	
1.[客户端网页API](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Client-side_web_APIs)    
2.[Web API简介](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Client-side_web_APIs/Introduction)      
3.[API Glossary](https://developer.mozilla.org/en-US/docs/Glossary/API)     
4.[XMLHttpRequest](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest) 		
5.[Fetch API](https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API)	 	
6.[创建动画循环](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/requestAnimationFrame)     
7.[操作文档](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Client-side_web_APIs/Manipulating_documents)		
8.[Axios](https://www.kancloud.cn/yunye/axios/234845)