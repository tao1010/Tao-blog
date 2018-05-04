---
title: HTML5-黑马视频-入门基础2
date: 2018-05-03 10:26:30
tags: html
categories: Web
---

一、表格			
1.创建表格			

```html
<table>
	<tr>
		<td>单元格内的文字</td>
		...
	</tr>
	...
</table>
解析:
	table用于定义一个表格；
	tr用于定义表格中的一行，必须嵌套在table标签中，table中包含几对tr，就表示几行表格；
	td用于定义表格中的单元格，必须嵌套在tr标签中，tr中包含几对td，就表示该行中有多少列，其中可以放任何标签；
```
2.表格属性		

|属性名|含义|常用属性值|
|---|---|---|
|border|设置表格的边框(默认border=0，无边框)|像素值|
|cellspacing|设置单元格与单元格之间的空白间距|像素值，默认2|
|cellpadding|设置单元格与单元格边框之间的空白间距|像素值，默认1|
|width|设置表格的宽度|像素值|
|height|设置表格高度|像素值|
|align|设置表格在网页中的水平对齐方式|left、center、right|
3.表头标签 th		

	将第一列的td-->th即可
4.表格结构			

```html
<table>
	<thead>
		...
	</thead>
	<tbody>
		...
	</tbody>
</table>
```
5.表格标题		
	
```html
<table>
	<caption>这是一个标题</caption>
	<thead>
		...
	</thead>
	<tbody>
		...
	</tbody>
</table>
```
6.合并单元格		
跨行rowspan,跨列colspan		

```html
<table border="1" align="center">
   <caption>这是一个标题</caption>
   <thead>
       <tr>
           <th colspan="2">第一行，第一列</th>
           <th>第一行，第三列</th>
         </tr>
   </thead>
   <tbody>
       <tr>
           <td>第二行，第一列</td>
           <td>第二行，第二列</td>
           <td rowspan="2">第二行，第三列</td>
        </tr>
        <tr>
           <td>第三行，第一列</td>
           <td>第三行，第二列</td>
           <!-- <td>第三行，第三列</td> -->
        </tr>
   </tbody>
</table>
 解析:
 	将多个内容合并的时候，把多余的东西删除；
 	删除个数 = 合并个数 - 1；
 	合并顺序：先上 先左；
 	
```
二、表单			
1.作用：收集用户信息		
2.构成:表单控件(表单元素)、提示信息、表单域	

	表单控件：按钮(单选按钮|复选按钮)、文本框|密码框、
	提示信息：
	表单域：
3.input控件	

|属性|属性值|描述|
|---|---|---|
|type|text|单行文本输入框|
|-|password|密码输入框|
|-|radio|单选按钮|
|-|checkbox|复选框|	
|-|button|普通按钮|
|-|submit|提交按钮|
|-|reset|重置按钮|
|-|image|图像形式的提交按钮|
| type |file|文件域|
|name|由用户自定义|控件的名称|
|value|由用户自定义|input控件中默认的文本值|
|size|正整数|input控件在页面中显示宽度|
|checked|checked|定义选择控件默认被选中的项|
|maxlength|正整数|控件允许输入的最多字符数|

解析：
	
	单选框，如果是一组，可以通过name值来实现：
	   <label for="sex">性别:</label>
	   <input type="radio" name="sex" id="sex"> 男
  	   <input type="radio" name="sex" id="sex"> 女
6.label标签				
作用：
	
	用于绑定一个表单元素			
使用方法：
	
	1）用label直接使用包裹；
		<label> 测试一下: <input type="text"> </label>
	2)label中有多个标签时，通过for 和 id 绑定
		<label for="name" >用户名:</label>
  		<input type="text" id="name" placeholder="请输入您的用户名"> 
7.文本域textarea
	
	<label for="leave">留言:</label>
    <textarea name="leave" id="leave" cols="30" rows="10"></textarea>
8.下拉菜单select
	
	select中至少包含一对option;
	默认选中时,在option中设置selected即可;
	
	<select name="city" id="city">
    	<option selected>北京</option>
    	<option>重庆</option>
    	<option>深圳</option>
  	</select>
9.表单域form

|属性|属性值|描述|
|---|---|---|
|action||提交的目的地|
|method|post/get|提交方式|
|name|字符||
三、HTML5新标签和特性		
1.常用新标签	
	
	header：定义页面的头部，页眉；
	nav：定义导航栏；
	footer：定义页面底部，页脚；
	article：定义文章；
	section：定义文档中的节；
	aside：定义侧边；
	datalist:定义选项列表；
	fieldset:将表单内的相关元素分组和打包，一般和legend搭配使用；
2.input新特性	

|属性|属性值|描述|
|---|---|---|
|type-H5|email|输入邮件格式|
|type-H5|tel|手机号格式|
|type-H5|url|url格式|
|type-H5|number|输入数字格式|
|type-H5|search|搜索框|
|type-H5|range|自由拖动滑块|
|type-H5|time|小时分钟|
|type-H5|date|年月日|
|type-H5|datetime|时间|
|type-H5|month|月年|
|type-H5|week|星期 年|	
3.常用input新属性		

|属性|属性值|描述|
|---|---|---|
|type-H5|placeholder|占位符|
|type-H5|autofocus|自动获得焦点|
|type-H5|multiple|多文件上传|
|type-H5|autocomplete|是否启用自动完成功能,on/off，on表示记录已经输入的值|
|type-H5|required|必填项，内容不能为空|
|type-H5|accesskey|激活(使元素获得焦点)元素的快捷键 采用alt+s|

	使用自动记录，需要提交按钮；需要name值；
	<input type="text" autocomplete name="username" placeholder="请输入名字">
	<input type="text" autocomplete=“off” name="username" placeholder="请输入名字">
四、多媒体标签		

	embed:标签定义嵌入的内容 - 不推荐 需要考虑兼容问题
	audio：播放音频
	video：播放视频
1.引入视频方式
	
	本地网页一般不存放视频，可以将其上传到优酷，然后在从优酷获取视频链接。		
2.声音			
属性：

	autoplay自动播放
	controls是否显示控件(播放、暂停、进度条、音量)
	loop循环播放 eg:loop="-1"无效循环，loop=“2”循环2次
兼容处理:
	
	一般提供三种格式: ogg mp3 wav做兼容处理，ogg+mp3能包含大部分浏览器。
	<audio controls autoplay>
		<source src="11.mp3" />
		<source src="11.ogg" />
	</audio>
	
3.视频			
支持格式：ogg mp4 webm
	
	<video src="11.mp4" autoplay controls width="200"></video>
兼容处理:
	
	<video controls autoplay>
		<source src="11.mp4" />
		<source src="11.ogg" />
		您的浏览器不支持视频播放
	</video>
	
参考资料:			
1.[W3C手册](http://w3school.com.cn)	


