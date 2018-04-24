---
title: HTML-表单-创建第一个表单
date: 2018-04-24 15:39:17
tags: html
categories: Web
---

一、HTML表单概念		
1.HTML表单是用户和Web站点或应用程序之间交互的主要内容之一.
	
	允许用户将数据发送到web站点;
	大多数情况数据被发送到web服务器;
	wenb页面也可以拦截数据并自己使用;
2.HTML表单是由一个或多个小部件组成.
	
	小部件包含:单行或多行、选择框、按钮、复选框、或单选按钮;
3.HTML表单和常规HTML文档主要区别:
		
	表单收集的数据发送到web服务器，需要设置web服务器来接收和处理数据。
二、设计表单		
![form-sketch-low](form-sketch-low.jpg)	   
1.&lt;form&gt;元素		
最佳实践：必须配置action和method属性
	
	action:定义了在提交表单时所收集的数据的位置(url).
	method:定义了发送数据的HTTP方法(get或post).
```html
 <form action="/my-handling-form-page" method="post">
 ...   
 </form>
```
2.&lt;label&gt;,&lt;input&gt;,&lt;textarea&gt;元素   

```html
<body>

  <form action="/my-handling-form-page" method="post">
    <div>
      <label for="name">Name:</label>
      <input type="text" id="name"/>
    </div>
    <div>
      <label for="mail">E-mail:</label>
      <input type="mail" id="mail"/>
    </div>
    <div>
      <label for="msg">Message:</label>
      <textarea name="msg" id="msg" cols="30" rows="10"></textarea>
    </div>
    <div class="button">
          <button type="submit">Send your message</button>
    </div>
  </form>
  <script src="html-basic.js"></script>
</body>
解析：	
	<div>元素可以方便构造代码,使样式更简单；
	<label>元素上使用for属性,将标签链接到表单小部件的一种正式方式；
	<input>元素中，最重要的是type属性，它定义了input属性的行为方式；
		默认值text，表示一个基本的单行文本字段，接受任何类型的文本输入；
		email，它定义了一个只接受格式良好的电子邮件地址的单行文本字段；
	注意<input  ... />和<textarea></textarea>的语法区别：
		<input   / > 标签是一个空元素，不需要关闭标签，获取值时必须使用value值；
		<textarea>不是一个空元素，必须使用适当的结束标记来关闭它；	
		 
```
3.&lt;button&gt;元素	

```html
<div class="button">
    <button type="reset">reset</button>
    <button type="submit">submit</button>
    <button>Send your message</button>
</div>
解析:	
	单击 submit 按钮 发送表单的数据到<form>元素的action 属性所定义的网页。
	单击 reset 按钮 将所有表单小部件重新设置为它们的默认值。从用户体验的角度来看，这被认为是一种糟糕的做法。
	单击button 按钮……不会发生任何事！这听起来很傻，但是用JavaScript构建定制按钮非常有用
```

<font color = 'red'>tips:</font>
	
	可以使用相应类型的 <input>元素来生成一个按钮，如 <input type="submit">,<input>元素只允许纯文本作为其标签。
	<button>元素的主要优点是，<button>元素允许完整的HTML内容，允许更复杂、更有创意的按钮文本。

三、表单基本样式		

```css
form {

    /* 表单居中 */
    margin: 0 auto;
    width: 400px;
    /* 显示轮廓 */
    padding: 1em;
    border: 1px solid #ccc;
    border-radius: 1em;
}

/*

A,B	匹配满足A（和/或）B的任意元素（参见下方”同一规则集上的多个选择器”）.
A B	匹配任意元素，满足条件：B是A的后代结点（B是A的子节点，或者A的子节点的子节点）
A > B	匹配任意元素，满足条件：B是A的直接子节点
A + B	匹配任意元素，满足条件：B是A的下一个兄弟节点（AB有相同的父结点，并且B紧跟在A的后面）
A ~ B	匹配任意元素，满足条件：B是A之后的兄弟节点中的任意一个（AB有相同的父节点，B在A之后，但不一定是紧挨着A）
*/
form div + div {

    margin-top: 1em;
}

label {

    /* 所有label大小相同正确对齐 */
    display: inline-block;
    width: 90px;
    text-align: right;
}

input, textarea{

    /* 确保所有文本输入框字体相同
     textarea默认是等宽字体 */
  font: 1em sans-serif;

  /* 使所有文本输入框大小相同 */
  width: 300px;
  box-sizing: border-box;

  /* 调整文本输入框的边框样式 */
  border: 1px solid #999;
}
input:focus, textarea:focus {
    /* 给激活的元素一点高亮效果 */
    border-color: #000;
  }
  textarea {
    /* 使多行文本输入框和它们的label正确对齐 */
    vertical-align: top;

    /* 给文本留下足够的空间 */
    height: 5em;
  }
  .button {
    /* 把按钮放到和文本输入框一样的位置 */
    padding-left: 90px; /* 和label的大小一样 */
  }
  
  button {
    /* 这个外边距的大小与label和文本输入框之间的间距差不多 */
    margin-left: .5em;
  }
```
四、向web服务器发送表单数据		

1.设置form标签的action和method属性，用以发送数据的位置和方式；    
2.需要为数据提供一个名称：	
	
	在浏览器：数据名称告知浏览器哪个名称提供每个数据；
	在服务器：数据名称允许服务器按照名称处理每个数据块；
3.将数据命名为表单，需要在每个表单小部件上使用name属性来收集特定的数据块：

```html
<form action="/my-handling-form-page" method="post"> 
  <div>
    <label for="name">Name:</label>
    <input type="text" id="name" name="user_name" />
  </div>
  <div>
    <label for="mail">E-mail:</label>
    <input type="email" id="mail" name="user_email" />
  </div>
  <div>
    <label for="msg">Message:</label>
    <textarea id="msg" name="user_message"></textarea>
  </div>

  ...

解析:
	表单会发送三个已命名的数据块 "user_name", "user_email", 和 "user_message";
	这些数据将用使用HTTP POST 方法,把信息发送到URL为 "/my-handling-form-page"目录下; 	
	在服务器端，位于URL"/my-handling-form-page" 上的脚本将接收的数据作为HTTP请求中包含的3个键/值项的列表;
 	
```	
	
参考资料：	
1.[HTML表单](https://developer.mozilla.org/zh-CN/docs/learn/HTML)		
2.[创建第一个表单](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Forms/Your_first_HTML_form)      
3.[关于表单用户体验](http://uxdesign.smashingmagazine.com/tag/forms/)		
4.[如何构造HTML表单的详细信息](https://developer.mozilla.org/en-US/docs/Learn/HTML/Forms/How_to_structure_an_HTML_form)	
5.[向web服务器发送表单数据](https://developer.mozilla.org/en-US/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data)