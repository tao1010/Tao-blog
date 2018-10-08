---
title: iOS其他-CocoaPods-应用
toc: true
date: 2018-07-02 09:36:49
tags: iOS其他
categories: iOS
---

CocoaPods是ruby写的，用于管理Xcode开发时第三方开源的工具。

	Mac自带Ruby环境
一、环境配置

<!-- more -->

1.Ruby软件源的处理
	
	查看Ruby版本：
		ruby -v
		
	验证Ruby源：
		gem source -l
	结果：
		*** CURRENT SOURCES ***

		https://ruby.taobao.org  #只要存在淘宝的源就可以安装
		https://gems.ruby-china.org
		
	添加Ruby源:
		gem sources -a https://ruby.taobao.org
	
	删除Ruby源:
		gem sources --remove https://ruby.taobao.org #删除淘宝ruby软件源
		gem sources --remove https://gems.ruby-china.org #删除China 社区ruby软件源
	
	更新Ruby:
		sudo gem update --system
2.安装CocoaPods到电脑
		
	方法一:推荐
		sudo gem install -n /usr/local/bin cocoapods 
	方法二:可能报错
		sudo gem install cocoapods 
	如果安装多个Xcode:
		sudo xcode-select -switch /Applications/Xcode.app/Contents/Developer
3.下载配置到本地
	
	pod setup #设置仓库，把所有上传到cocoapods的第三方框架下载每个版本和网络地址以及一些其他描述信息到本地
二、CocoaPods的使用<br>
1.查找第三方
	
	pod search xxx
	eg:
		pod search AFNetworking
2.创建Podfile文件
	
	cd  xxx/xxx #切换到项目文件夹下
	方法一:
		touch Podfile #新建文件Podfile
	方法二：
		pod init #会自动配置 Podfile文件内容，编辑时只需添加第三方即可
3.编辑Podfile文件
	
	方法一：
		vim Podfile    #进入编辑界面
		i			   #开始编辑
		键盘 ‘esc’      #退出编辑
		：wq			   #写入完成保持并退出编辑界面
	方法二：
		open Podfile    #会用文本编辑器打开
4.Podfile文件内容

    写法1：
  	  	target 'MyApp' do   #针对MyApp项目导入依赖库
	  		 pod 'xxx','~>1.0'
	   end
	   
	 写法2:
	 	# 下面两行是指明依赖库的来源地址
		source 'https://github.com/CocoaPods/Specs.git'
		source 'https://github.com/Artsy/Specs.git'
		
		# 说明平台是ios，版本是9.0
		platform :ios, '9.0'
		
		# 忽略引入库的所有警告（强迫症者的福音啊）
		inhibit_all_warnings!
		
		# 针对MyApp target引入AFNetworking
		# 针对MyAppTests target引入OCMock，
		target 'MyApp' do 
		    pod 'AFNetworking', '~> 3.0' 
		    target 'MyAppTests' do
		       inherit! :search_paths 
		       pod 'OCMock', '~> 2.0.1' 
		    end
		end
5.安装CocoaPods到项目

	pod install
三、其他<br>
1.Podfile中的依赖项

	pod 指定特定的依赖库
	podspec 可以提供一个API来创建podspecs
	target 通过target指定依赖范围
	
	source 指定pod来源 默认使用CocoaPods官方Source，建议默认
2.依赖库版本问题
	
	#指定依赖库具体版本格式：
	pod 'AFNetworking', '3.0' 
	
	#不指定依赖库版本，默认选取最新版本:
	pod 'AFNetworking'
	
	#指定版本范围
	pod 'AFNetworking', '> 3.0' #高于3.0的任意一个版本
	pod 'AFNetworking', '<=3.0' #低于(包含)3.0的任意一个版本
	pod 'AFNetworking', '～>3.1.2' #版本3.1.2 到 版本3.2(不包含)之间的任意一个版本
3.use_frameworks!
	
	use_frameworks使用的区别:
		使用 use_frameworks! 命令会在Pods工程下生成Frameworks目录下生成依赖库的framework
		不使用 use_frameworks! 命令会在Pods工程下的Products目录下生成.a 的静态库
推荐使用 use_frameworks！纯OC项目一般不使用 swift必须使用 use_frameworks! 
	
||优点|缺点|
|---|---|---|
|静态库(静态链接库 .a)|在编译时会将库copy一份到目标程序中，编译完成之后，目标程序不依赖外部的库，也可以运行|会使应用程序变大|
|动态库(.dylib)|编译时只存储了指向动态库的引用;可以多个程序指向这个库，在运行时才加载，不会使体积变大|运行时加载会损耗部分性能，并且依赖外部的环境，如果库不存在或者版本不正确则无法运行|
| Framework |实际上是一种打包方式，将库的二进制文件，头文件和有关的资源文件打包到一起，方便管理和分发| 暂无 |
4.pod install 和 pod update区别

	pod install 下载并安装Pod
		当podfile 文件中 有 “增、删、改 pod”的操作后使用；
		pod install 执行完后将会下载依赖库的版本号添加到podfile.lock文件；
		pod install 根据podfile.lock文件列出的已安装的pod版本信息，只负责下载安装podfile.lock中不存在的pod，不会自动更新已安装的pod版本；
	pod update 更新已存在Pod
		按规则将podfile文件中的pod更新到最新版本，并将pod版本信息写入podfile.lock;		
四、错误<br>
1.While executing gem ...
	
	复现:
		执行命令 sudo gem install cocoapods 报错  
	处理:
		sudo gem install -n /usr/local/bin cocoapods
2.[!] Oh no, an error occurred.
Search for existing github issues similar to yours:
...
	
	解决方案:
	sudo rm -rf ~/.cocoapods/repos/master
	pod setup		
		

参考资料:<br>
1.[Podfile配置](https://www.jianshu.com/p/8a0fd6150159)<br>
2.[pod install 和 pod update区别](https://blog.csdn.net/cwf19860527/article/details/54139214)


