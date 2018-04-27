---
title: HTML-表单-表单数据发送
date: 2018-04-27 09:15:08
tags: html
categories: Web
---

一、数据去向		
1.客户端和服务端体系结构		
![client-server](client-server.png)		
客户端(通常是web浏览器)向服务器(大多数情况下事Apache、Nginx、IIS、Tomcat等web服务器)发送请求,使用HTTP协议,服务器使用相同的协议来回答请求。    
在客户端，HTML表单是一种方便的用户友好方式,可以配置HTTP请求将数据发送到服务器，使用户能够在HTTP请求中传递信息。     
2.在客户端定义如何发送数据		
&lt;form&gt;元素定义了如何发送数据,它所有属性都是为了配置当用户点击提交按钮时发送的请求:
	
2.1属性action:定义发送数据位置,它必须是一个有效的URL。如果没有提供此属性，数据将被发送到包含表单的页面的URL。

	eg1:数据被发送到一个绝对的URL：http://foo.com
		<form action = 'http://foo.com'>
	eg2:数据被发送到服务器上不同的URL
		<form action = '/somewhere_else'>
	eg3:没有属性，数据被发送到表单出现的相同页面上
		<form >
	eg4:较老的页面，数据被发送到包含表单的相同页面
		<form action = '#'>
		
	可以指定HTTPS协议的URL，数据将与请求的其余部分一起加密(即使表单本身是托管在使用HTTP访问的不安全页面上)，如果表单托管在安全页面上，但指定了一个不安全的HTTP URL,带有action属性，所有浏览器都会在每次尝试发送数据时向用户显示一个安全警告,因为数据不会被加密。
		
2.2属性method:定义发送数据的方式，常用的GET和POST
		
	HTTP工作原理:每当访问Web上资源时，浏览器都会向URL发送一个请求。HTTP请求包含2部分：一个包含关于浏览器功能的全局数据集的数据头，一个包含服务器处理特定请求所需信息的主体。
GET方法：请求服务器返回给定的资源。浏览器发送一个空的主体，如果使用GET方法发送一个表单，那么发送到服务器的数据将被追加到URL。

```html
//HTML
<form action="http://foo.com" method="get">
    <div>
      <label for="say">What greeting do you want to say?</label>
      <input name="say" id="say" value="Hi">
    </div>
    <div>
      <label for="to">Who do you want to say it to?</label>
      <input name="to" value="Mom">
    </div>
    <div>
      <button>Send my greetings</button>
    </div>
</form>
解析:	
	点击按钮提交表单时，在浏览器地址栏中，地址为:www.foo.com/?say=Hi&to=Mom;
	数据被附加到URL作为一系列的名称/值对(name/value);
	URL Web地址结束之后： /?name1=value1&name2=value2 的方式组合
		GET	:/?say=Hi&to=Mon  HTTP/1.1
		Host: foo.com
```		
POST方法：浏览器在请求响应时，需要考虑到HTTP请求体中提供的数据，使用POST方法发送表单，则将数据追加到HTTP请求的主体中。

```html
<span>POST方法
	<form action="http://foo.com" method="post">
		<div>
  			<label for="say">What greeting do you want to say?</label>
  			<input name="say" id="say" value="Hi">
		</div>
		<div>
			 <label for="to">Who do you want to say it to?</label>
			 <input name="to" value="Mom">
		</div>
		<div>
 			 <button>Send my greetings</button>
		</div>
	</form>
</span>
解析:	
	当使用POST方法提交表单时，将没有附加到URL的数据，请求主体中包含的数据：
		POST / HTTP/1.1
		Host: foo.com
		Content-Type:application/x-www-form-urlencoded
		Content-Length:13
		say=Hi&to=Mon
	Content-Length数据头表示主体的大小，Content-Type数据头表示发送到服务器的资源类型。
```
![getpost](getpost.png)			

使用GET请求用户将在它们的URL栏中看到数据，使用POST请求用户将不会看到：
	
	如果需要发送一个密码(或其他敏感数据),永远不要使用GET方法否则数据会在URL栏中显示，非常不安全;
	如果需要发送大量的数据，POST方法是首选，因为一些浏览器限制了URL的大小，许多服务器限制了接受的URL长度.
3.在服务器端：检索数据			
无论选择哪种HTTP方法，服务器都会接收一个字符串并解析它以获取作为键/值对序列的数据。您访问这个序列的方式取决于您使用的开发平台以及您可能使用的任何特定框架。您使用的技术也决定了如何处理密钥副本；通常，最近收到的密钥的值是优先的。

eg1:原始PHP

eg2:Python

eg3:其他语言和框架		
其他语言:

	Perl
	Java
	.Net
	Ruby
框架：

	PHP的Symfony
	Python的Django（比Flask要重一些，但是有更多的工具和选项。）
	Node.js的Express
	Ruby的Ruby On Rails
	Java的Grails
	...
二、特殊案例:发送文件		
用HTML发送文件是一个特殊的例子。文件是二进制数据(或者被认为是这样)，而所有其他数据都是文本数据。由于HTTP是一种文本协议，所以处理二进制数据由特殊的要求。	

enctype属性：允许指定在提交表单时所生成的请求在Content-Type的<font color="red">HTTP数据头的值(非常重要)</font>，这个数据告诉服务器正在发送什么数据,默认情况下，值为:application/x-www-form-urlencoded(意思是:这是已编码为URL参数的表单数据)。

发送文件的三个额外步骤:
	
	将method属性设置为POST，因为文件内容不能放入URL参数中。
	将enctype的值设置为multipart/form-data，因为数据将被分成多个部分，每个文件分别对应一个文件以及表单正文中包含的文本数据(如果文本也输入到表单中)。
	包含一个或多个File picker小部件，允许用户选择将要上传的文件。

```html
<span>上传文件
    <form method="post" enctype="multipart/form-data">
      <div>
        <label for="file">Choose a file</label>
        <input type="file" id="file" name="myFile">
      </div>
      <div>
        <button>Send the file</button>
      </div>
    </form>
</span>
```	
许多服务器配置了文件和HTTP请求的大小限制，发送文件之前，请检查服务器管理员权限。			
三、常见的安全问题	

HTML表单是最常见的攻击媒介，因此每次向服务器发送数据时，都需要考虑安全性。    
1.脚本注入攻击：当你将用户发送的数据显示给用户或另一个用户时，攻击可能在此时发生.
	
	XSS:跨站脚本,将客户端脚本注入到其他用户查看的Web页面中。攻击者可以使用跨站点脚本攻击的漏洞来绕过诸如同源策略之类的访问控制;
		利用用户对web站点的信任;
	CSRF:跨站点请求伪造,试图将特权升级到特权用户(比如站点管理员)的权限，以执行他们不应该执行的操作(例如，将数据发送给一个不受信任的用户);
		利用网站为其用户提供的信任;
		
	防范：始终检查用户发送给服务器的数据(如果需要显示)，尽量不要显示用户提供的HTML内容。
		  应该处理用户提供的数据，不要逐字地显示它。
		  当今市场上几乎所有的框架都实现了一个最小的过滤器，它可以从任何用户发送的数据中删除HTML<script>、<iframe> 和<object> 元素。
		  这有助于降低风险，但并不一定会消除风险。
2.SQL注入		
	
	一种试图在目标web站点使用的数据库上执行操作的攻击类型；
	当服务器存储由用户发送数据时,发生攻击，它可以导致数据丢失，甚至使用特权升级控制整个网络基础设施的攻击;
	防范：永远不应该存储用户发送的数据而不执行一些清理工作(PHP/mysql基础设施上使用mysql_real_escape_string()).
3.HTTP数据头注入和电子邮件注入
	
	一种在应用程序基于表单上用户数据的输入构建HTTP头部或电子邮件时出现此类攻击；
	会话劫持或者网络钓鱼攻击；
4.偏执：永远不要相信你的用户		
所有到达服务器的数据都必须经过检查和消毒：
	
	有潜在危险的字符转义。应该如何谨慎使用的特定字符取决于所使用的数据的上下文和所使用的服务器平台，但是所有的服务器端语言都有相应的功能。
	限制输入的数据量，只允许有必要的数据。
	沙箱上传文件(将它们存储在不同的服务器上，只允许通过不同的子域访问文件，或者通过完全不同的域名访问文件更好)。
			
参考资料：	
1.[表单](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Forms)      
2.[发送表单数据](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data)    
3.[服务器端网站编程的第一个步骤](https://developer.mozilla.org/en-US/docs/Learn/Server-side/First_steps)     
服务器框架			
4.[PHP的Symfony](http://symfony.com/)	  
5.[Python的Django](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Django)    
6.[Flask](http://flask.pocoo.org/)		
7.[Node.js的Express](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Express_Nodejs)     
8.[Ruby的Ruby On Rails](http://rubyonrails.org/)			
9.[Java的Grails](http://grails.org/)	    
10.[File_picker](https://developer.mozilla.org/en-US/docs/Learn/HTML/Forms/The_native_form_widgets#File_picker)    
11.[服务器端安全](https://developer.mozilla.org/en-US/docs/Learn/Server-side)	
