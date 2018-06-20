---
title: 编译和打包Web
date: 2018-06-12 08:55:09
tags: web-build
categories: Web
---

一、Node<br>

<!--more-->

1.安装[Node](https://nodejs.org)<br>
	
	Node和npm会 默认安装在/usr/local/bin目录下
2.命令终端退出<br>

	1.control + d - 当命令出现时使用(ctrl+d)
	2.control + c - 当命令在运行时，只有光标闪烁时使用(ctrl+c)
二、步骤(编译和打包)<br>
![build](build.jpg)<br>
工程快捷命令:

	查看目录文件中 package.json 文件中 key: 'scripts' 所对应的值：
	 "scripts": {
	    "dev": "webpack-dev-server --inline --progress --config build/webpack.dev.conf.js",
	    "start": "npm run dev",
	    "build": "node build/build.js",
	    "http": "node ./service/http/http.js"
	  }
	  npm run start <==> npm run dev
	  npm run build <===> node build/build.js
	  npm run http <===> node ./service/http/http.js
1.安装node<br>

	查看node版本： node --version	
	查看npm(node的包管理器)版本: npm -v
2.配置npm相关文件文件
	
	npm install
3.cd到项目目录下：
	
	cd xxx/xxx/xxx/2.0
4.在生产环境运行项目
	
	npm run start
	命令结束：
		...
		DONE  Compiled successfully in 18125ms 
		Your application is running here: http://localhost:8080
5.在浏览器中输入步骤4完成时提供的IP：

	http://localhost:8080
6.编译
	
	npm run build
	命令结束:
		Build complete.
	   Tip: built files are meant to be served over an HTTP server.
	   Opening index.html over file:// won't work.
		
		在工程目录下生成dist文件夹
7.将dist文件夹中的内容拷贝到发布的路径中即可发布。

8.开辟本地服务器运行项目
	
	npm run http
	命令结束：
		start http://localhost:9090
		在浏览器中输入IP即可查看项目
