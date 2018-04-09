---
title: Hexo+Github搭建自己的博客
date: 2018-03-02 11:32:25
tags: hexo
categories: 博客
toc: true

---

一、本地化

1.安装Node.js

作用：用来生成静态界面，可去官网下载

2.安装git

作用:用来将本地Hexo内容提交到Github上。Xcode自带Git，这里不再赘述。如果没有Xcode可以参考Hexo官网上的安装方法。

3.安装Hexo

ps：必须是在1，2步骤完成后才能正式安装Hexo

``` bash
sudo npm install -g hexo
```

(输入管理员密码即可开始安装  sudo: linux系统管理指令     -g:全局安装)

4.初始化本地信息

4.1 终端： 
	
	cd 到指定目录下

4.2 创建自己的文件夹

``` bash
hexo init blog      //blog是你建立的文件夹名称
```

或者
``` bash
hexo init
```

4.3 终端： 

	cd 到blog文件夹下

4.4安装npm：
``` bash
npm install
```
4.5开启hexo服务器：
``` bash
hexo s
```
ps：此时，在浏览器中打开网址：http://localhost:4000能看到Hexo的网页信息

二、github配置

1.关联Github

2.登录github，新建仓库，固定写法：“用户名.github.io”

2.1本地blog文件夹中（_config.yml db.js node_modules package.json scaffolds source themes）
终端：
	
	cd 到blog文件夹下

2.2终端：vim  _config.yml

滑到最后修改如下：

	deploy:
	type: git
	repository: https://github.com/TonnyTeng/tao.github.io.git
	branch: master
需要将repository后的url换成你自己的url

ps：所有配置（_config.yml  theme ）中，所有的“：”后边都要加一个空格（至少一个），否则执行hexo命令会报错

2.3在blog文件夹目录下执行生成静态页面的命令：
``` bash
hexo generate 或者 hexo g
```

2.5 执行配置命令：
``` bash
hexo deploy 或者hexo d
```
ps：若执行命令hexo deploy仍然报错：无法连接git或找不到git，则执行如下命令来安装hexo-deployer-git：
``` bash
npm install hexo-deployer-git --save   //在blog文件夹下
```
再次执行 5.4 + 5.5

2.6输入github账号密码

ps：为避免每次输入账号密码，参考步骤四


三、发布文章

1.cd 到blog文件夹下
``` bash
hexo new postName
```
ps:名为postName.md的文件会建在目录/blog/source/_posts下，postName是文件名，为方便链接不建议掺杂汉字。可以用vim编辑文章，也可以用MarkDown编辑

2.生成静态页面
``` bash
hexo generate 或者 hexo g
```
3.将文章部署到Github
``` bash
hexo deploy 或者 hexo d
```
4.终端：hexo clean

ps：每次发布内容命令如下：

4.1. hexo clean

4.2 hexo g

4.3 hexo d

四、关联账号密码-添加ssh key 到github

待验证...

五、安装主题

1.选择主题 官网主题

eg:https://github.com/jangdelong/hexo-theme-xups.git

2.cd 到blog文件夹下的themes文件夹下执行命令：

	git clone https://github.com/jangdelong/hexo-theme-	xups.git  //将主题下载到本地

3.配置_config.yml的theme配置
	
	theme: hexo-theme-xups

4.终端：
	
	hexo s --watch

5.cd到blog文件夹下

	hexo g 或者 hexo generate

6.终端：
	
	hexo s --watch
7.添加背景音乐

步骤：
	
	a.打开网易云音乐网页版http://music.163.com/；	b.搜索歌曲,选择歌曲双击进入，点击生成外链播放器，拷贝HTML代码；
	c.打开主题路径如下:layout-->_partial-->left-col.ejs;
	d.将b拷贝内容粘贴到</nav>上面；
	e.改变播放器尺寸 width=260 height=86
	
	
	

六、域名解析

待验证...

七、配置信息 _config.yml

Site

	title: Tao的博客
	subtitle:
	description:
	author: Tao
	language: zh-Hans
	timezone:
参考资料：

1.https://hexo.io/docs/

2.https://nodejs.org/en/

3.http://gonghonglou.com/2016/02/03/firstblog/

ps:

1.node 终端命令：

	退出终端： .exit
参考资料：

1.[博客系统创建菜单](https://blog.csdn.net/chwshuang/article/details/52350518)   
2.[hexo统计插件](http://tengj.top/2016/03/17/hexo7count/)   
3.[Hexo+yilia主题实现文章目录和添加视频](https://blog.csdn.net/u013082989/article/details/70212008)  
4.[Hexo-yilia主题修改](http://lawlite.me/2017/04/17/Hexo-yilia主题实现文章目录和添加视频/)  
5.[yilia主题添加背景音乐](https://www.jianshu.com/p/9a3fc2bdfac7)