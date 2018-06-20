---
title: JavaScript基础-客户端WebAPI-从服务器获取数据
date: 2018-04-24 09:10:33
tags: JavaScript
categories: Web
---

一、更新页面数据

<!--more-->
		
问题描述：创建允许页面请求小块数据(HTML、XML、JSON或纯文本)仅在需要时显示它们.     
1.如果加载整个页面数据
	
	浪费带宽资源，用户体验差;
	页面越来越多，越来越复杂;
2.加载更新部分内容
	
	性能更优(减少带宽的浪费)，刷新速度快，响应快，用户体验更好;
3.解决方案使用技术:		
Ajax(Asynchronous JavaScript and XML) - XMLHttpRequest 和 Fetch API(这是通过使用诸如 XMLHttpRequest 之类的API或者 — 最近以来的 Fetch API 来实现)      
方法一：网页直接处理对服务器上可用的特定资源的 HTTP 请求，并在显示之前根据需要对结果数据进行格式化。      
![moderne-web-site-architechture](moderne-web-site-architechture.png)    
方法二：首次请求时将资源和数据存储在用户计算机上，后续访问中使用本地信息资源，内容仅在更新后从服务器重新加载。     
![web-app-architecture](web-app-architecture.png)    
二、基本的Ajax请求		
1.XMLHttpRequest		

```html
//html

<head>
  <meta charset="UTF-8">
  <title>Ajax starting point</title>
  <link rel="stylesheet" href="js-demo.css">
</head>
<body>
  <h1>Ajax starting point</h1>

  <form>
    <label for="verse-choose">Choose a verse</label>
    <select id="verse-choose" name="verse-choose">
      <option>Verse 1</option>
      <option>Verse 2</option>
      <option>Verse 3</option>
      <option>Verse 4</option>
    </select>
  </form>

  <h2>The Conqueror Worm, <em>Edgar Allen Poe, 1843</em></h2>

  <pre>

  </pre>
  <script src="js-demo.js"></script>
</body>
```
```css
//css

html, pre {
  font-family: sans-serif;
}
body {
  width: 500px;
  margin: 0 auto;
  background-color: #ccc;
}
pre {
  line-height: 1.5;
  letter-spacing: 0.05rem;
  padding: 1rem;
  background-color: white;
}
label {
  width: 200px;
  margin-right: 33px;
}
select {
  width: 350px;
  padding: 5px;
}
```
```js
//js

var verseChoose = document.querySelector('select');
var poemDisplay = document.querySelector('pre');
verseChoose.onchange = function (){

    var verse = verseChoose.value;
    updateDisplay(verse);
}

function updateDisplay(verse){

    verse = verse.replace(" ","");//去掉空格
    verse = verse.toLowerCase();//将大写转化为小写
    var url = verse + '.txt';//字符串拼接

    //方法一:创建XHR请求
    var request = new XMLHttpRequest();
    request.open('GET',url);
    request.responseType = 'text';
    request.onload = function(){

        poemDisplay.textContent = request.response;
    }
    request.send();


    //方法二:创建Fetch请求
    fetch(url).then(function(response){
        response.text().then(function(text){
            poemDisplay.textContent = text;
        });
    });
}
updateDisplay('Verse 1');
verseChoose.value = 'Verse 1';
```
	
	
	
参考资料：	
1.[客户端网页API](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Client-side_web_APIs)    
2.[从服务器获取数据](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Client-side_web_APIs/Fetching_data)      
3.[Axios](https://www.kancloud.cn/yunye/axios/234845)    
4.[XMLHttpRequest](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest)   
5.[Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)		