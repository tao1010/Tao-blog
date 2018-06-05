---
title: iOS面试-网络
date: 2017-02-02 10:26:25
tags: 面试
categories: iOS
---

一、TCP连接			
![osi](osi.png)		
1.概念	
	
	TCP是一种面向连接的、可靠的，基于字节流的传输层通信协议;
	TCP工作在网络OSI七层模型只能够的第四层-传输层;
2.TCP的三次握手				
![tcp_3](tcp_3.png)		

	简述：
	1.客户端发送报文（SYN=1，seq=x），进入SYN_SEND状态，等待服务器响应；
	2.服务器确认报文(SYN=1,seq=y,ACK=1,Ack N=x+1)，发送报文，进入接受状态;
	3.客户端确认响应，发送报文(SYN=0,ACK=1,Ack N=y+1)，建立客户端和服务器连接；
	
	详细描述：
	1.客户端发送SYN标志位为1，Sequence Number为x的连接请求报文段，然后客户端进入SYN_SEND状态，等待服务器的确认响应;
	2.服务器收到客户端的连接请求，对这个SYN报文段进行确认，然后发送Acknowledgment Number为x+1(Sequence Number+1)，SYN标志位和ACK标志位均为1，Sequence Number为y的报文段（即SYN+ACK报文段）给客户端，此时服务器进入SYN_RECV状态;
	3.客户端收到服务器的SYN+ACK报文段，确认ACK后，发送Acknowledgment Number为y+1，SYN标志位为0，ACK标志位为1的报文段，发送完成后，客户端和服务器端都进入ESTABLISHED状态;
	established
	完成TCP三次握手，客户端和服务器端成功地建立连接，可以开始传输数据;
3.TCP的四次挥手		
![tcp_4](tcp_4.png)				
	
	简述：
	1.客户端发送FIN报文(Seq=x+2,Ack N=y+1)，进入FIN_WAIT_1状态;
	2.服务器接收FIN报文，发送ACK报文(Ack N=Seq+1=x+3),客户端进入FIN_WAIT_2状态;
	3.服务器发送FIN报文(Seq=y+1)，进入LAST_ACK状态;
	4.客户端接收FIN报文，发送ACK报文(Ack N=Seq=1=y+2)，进入FIN_WAIT状态;服务器接收ACK报文，进入CLOSED状态，客户端等待未收到回复，进入CLOSED状态;
	
	详细描述：
	1.客户端发送Sequence Number为x+2，Acknowledgment Number为y+1的FIN报文段，客户端进入FIN_WAIT_1状态，即告诉服务端没有数据需要传输了，请求关闭连接；
	2.服务端收到客户端的FIN报文段后，向客户端应答一个Acknowledgment Number为Sequence Number+1的ACK报文段，即应答客户端你的请求我收到了，但是我还没准备好，请等待我的关闭请求。客户端收到后进入FIN_WAIT_2状态；
	3.服务端完成数据传输后向客户端发送Sequence Number为y+1的FIN报文段，请求关闭连接，服务器进入LAST_ACK状态；
	4.客户端收到服务端的FIN报文段后，向服务端应答一个Acknowledgment Number为Sequence Number+1的ACK报文段，然后客户端进入TIME_WAIT状态；服务端收到客户端的ACK报文段后关闭连接进入CLOSED状态，客户端等待2MSL后依然没有收到回复，则证明服务端已正常关闭，客户端此时关闭连接进入CLOSED状态
4.TCP、UDP、HTTP、Socket的区别
	
	socket是对TCP/IP协议的封装和应用,让我们更简单的使用TCP/IP协议;
	TCP/IP协议是传输层协议，主要解决数据如何在网络中传输;
	HTTP是应用层协议，主要解决如何包装数据;
	

二、HTTP连接		
1.概念		

	HTTP协议:超文本传送协议(HTTP：Hypertext Transfer Protocol)
		是保证客户端和服务器之间的通信；
		是建立在TCP协议上的一种应用；
	HTTP连接：最显著的特点是客户端发送的每次请求都需要服务器回送响应，在请求结束后，会主动释放连接。从建立连接到关闭连接的过程称为“一次连接”。	由于HTTP在每次请求结束后都会主动释放连接，因此HTTP连接是一种“短连接”；
	要保持客户端程序的在线状态，需要不断地向服务器发起连接请求。通常的做法是即时不需要获得任何数据，客户端也保持每隔一段固定的时间向服务器发送一次“保持连接”的请求，服务器在收到该请求后对客户端进行回复，表明知道客户端“在线”。
	若服务器长时间无法收到客户端的请求，则认为客户端“下线”，若客户端长时间无法收到服务器的回复，则认为网络已经断开；
2.GET和POST

||GET|POST|
|---|---|---|
|是否缓存|YES|NO|
|是否保存在浏览器历史记录|参数会保留记录中|参数不会保留记录中|
|是否可做书签|YES|NO|
|安全性|所发送的数据是 URL 的一部分<br>在发送密码或其他敏感信息时绝不要使用 GET |参数不会被保存在浏览器历史或 web 服务器日志中|
|是否有长度限制|YES 255字节|NO|
|使用场景|获取数据||
|传参方式|使用URL或Cookie|将数据放在Body中|
|查询字符串(key/value)|请求的URL中发送的|请求的HTTP消息主体中发送的|
|后退或刷新||数据重新提交|
|编码类型|application/x-www-form-urlencoded|application/x-www-form-urlencoded 或 multipart/form-data。为二进制数据使用多重编码|
|数据类型|只允许 ASCII 字符|没有限制。也允许二进制数据|
|可见性|数据在URL中对所有人可见|数据不显示字啊URL中|

三、JSON和XML		
1.概念
	
	JSON(JavaScript Object Notation):
		是一种轻量级的数据交换格式，具有良好的可读性和便于快速编写的特性。
		可在不同平台之间进行数据交换；
		采用兼容性很高的、完全独立于语言文本格式，同时具备类似C语言的习惯体系的行为，这些特性使其成为理想的数据交换语言。
	XML:
		扩展标记语言(Extensible Markup Language)；
		用于标记电子文件使其具有结构性的标记语言，可以用来标记数据，定义数据类型；
		是一种允许用户对自己的标记语言进行定义的源语言；
		使用DTD(document type definition)文档类型定义来组织数据，格式统一，跨平台和语言；
		合适Web传输，提供统一的方法来描述和交换独立于应用程序或供应商的结构化数据；
2.优缺点

||XML|JSON|
|---|---|---|
|优点|1.格式统一，符号标准 <br>2.容易与其它系统进行远程交互，数据共享比较方便|1.数据格式比较简单，易于读写，格式都是压缩的，占用带宽小<br>2.易于解析，客户端JavaScript可以简单的通过eval()进行JSON数据的读取<br>3.支持多种语言，包括ActionScript, C, C#, ColdFusion, Java, JavaScript, Perl, PHP, Python, Ruby等服务器端语言，便于服务器端的解析<br>4.在PHP世界，已经有PHP-JSON和JSON-PHP出现了，偏于PHP序列化后的程序直接调用，PHP服务器端的对象、数组等能直接生成JSON格式，便于客户端的访问提取<br>5.JSON格式能直接为服务器端代码使用，大大简化了服务器端和客户端的代码开发量，且完成任务不变，并且易于维护|
|缺点|1.文件庞大，文件格式复杂，传输占带宽<br>2.服务器端和客户端需要花费大量代码来解析XML，导致服务器端和客户端代码变得异常复杂且不易维护<br>3.客户端浏览器之间解析XML方式不一致，需要重复编写很多代码<br>4.服务器端和客户端解析XML花费较多资源和时间|1.没有XML格式这么推广的深入人心和喜用广泛，没有XML那么通用性<br>2.JSON格式目前在Web Service中推广还属于初级阶段|
|可读性|可读性较好||
|可扩展性|XML都能扩展|部分能扩展|
|编码难度|编码工具多，难度大|编码难度小|
|解码难度|难道大|非常容易|
|流行度|广泛使用|刚开始，潜力无限|
|解析手段|||
|数据体积|数据大，传输慢|数据小，传输快|
|数据交互||与JS的交互更方便，易解析|
|数据描述|好|差|
|使用场景|合适大数据的处理|解析较少数据|

参考资料：	
1.[TCP](https://www.jianshu.com/p/c2cbf6dc09a2)		
2.[XML和JSON区别](http://www.cnblogs.com/SanMaoSpace/p/3139186.html)	
3.[Socket和Http的区别](https://blog.csdn.net/zeng622peng/article/details/5546384)		
4.[GET和POST区别](https://www.jianshu.com/p/45c30b593872)
  [GET和POST区别+Demo](https://blog.csdn.net/heyddo/article/details/37561005)

	
