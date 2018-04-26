---
title: HTML-表单-构造HTML表单
date: 2018-04-25 09:11:03
tags: html
categories: Web
---

一、表单元素	
1.&lt;form&gt;元素		
 &lt;form&gt; 元素按照一定的格式定义了表单和确定表单行为的属性。想要创建一个HTML表单时，都必须从这个元素开始，然后把所有内容都放在里.
 	
 	严格禁止在一个表单内嵌套另一个表单，嵌套会使表单的行为不可预知，这取决于使用的浏览器;
 	表单小部件可以在form表单之外使用，但是这样与form表单五任何关系,必须使用JS定制它们的行为;
 	HTML5在HTML表单元素中引入了form属性,显示的将元素和表单绑定在一起，即使元素不在form中.
2.&lt;fieldset&gt;和&lt;legend&gt;元素	  
 
 	<fieldset>元素是一种方便的用于创建具有相同目的的小部件组的方式，出于样式和语义目的。 
 	可以在<fieldset>开口标签后加上一个 <legend>元素来给<fieldset> 标上标签。 
 	<legend>的文本内容正式地描述<fieldset>的用途。它是包含在<fieldset>里的。
	许多辅助技术将使用<legend> 元素，就好像它是相应的 <fieldset> 元素。
 
eg:			
![form](form.png)		
 
 ```html
 <form>
  <fieldset>
    <legend>Fruit juice size</legend>
    <p>
      <input type="radio" name="size" id="size_1" value="small">
      <label for="size_1">Small</label>
    </p>
    <p>
      <input type="radio" name="size" id="size_2" value="medium">
      <label for="size_2">Medium</label>
    </p>
    <p>
      <input type="radio" name="size" id="size_3" value="large">
      <label for="size_3">Large</label>
    </p>
  </fieldset>
</form>
解析:	
	每当有一组单选按钮时，应该将它们嵌套在<fieldset>元素中;
	一般来说，<fieldset>元素也可以用来对表单进行分段;
	长表单应该在多个页面之间进行拆分，但是如果表单很长，但必须在单个页面上，那么在不同的fieldsets中放置不同的相关部分可以提高可用性
	因为它对辅助技术的影响， <fieldset> 元素是构建可访问表单的关键元素之一。
	如果可能，每次构建表单时，尝试侦听屏幕阅读器如何解释它。如果听起来很奇怪，试着改进表单结构.
 ```
 3.&lt;label&gt;元素		
 
 	 <label> 元素是为HTML表单小部件定义标签的正式方法;
 	 设置for属性很重要;
 标签也可点击		
 	
 	对于单选按钮和复选框特别有用——这种控件的可点击区域可能非常小，设置标签来使它们可点击区域变大是非常有用的;
 eg:
 	
 ```html
 <form>
  <p>
    <label for="taste_1">I like cherry</label>
    <input type="checkbox" id="taste_1" name="taste_cherry" value="1">
  </p>
  <p>
    <label for="taste_2">I like banana</label>
    <input type="checkbox" id="taste_2" name="taste_banana" value="2">
  </p>
</form>
解析:
	label标签的for属性和input
	
 ```	
 
 多个标签		
 
 	严格地说，可以在一个小部件上放置多个标签，但不推荐，因为一些辅助技术可能难以处理它们。在多个标签的情况下,应该将一个小部件和它的标签嵌套在一个<label>元素中
 eg:
 
 ```html
 <p>Required fields are followed by <abbr title="required">*</abbr>.</p>

<!-- So this: -->
<div>
  <label for="username">Name:</label>
  <input type="text" name="username">
  <label for="username"><abbr title="required">*</abbr></label>
</div>

<!-- would be better done like this: -->
<div>
  <label for="username">
    <span>Name:</span>
    <input id="username" type="text" name="username">
    <abbr title="required">*</abbr>
  </label>
</div>

<!-- But this is probably best: -->
<div>
  <label for="username">Name: <abbr title="required">*</abbr></label>
  <input id="username" type="text" name="username">
</div>
 ```
 二、表单的通用HTML结构	
 1.用&lt;div&gt;元素包装标签和它的小部件是很常见的做法。&lt;p&gt;元素也经常被使用，HTML列表也是如此（后者在构造多个复选框或单选按钮时最为常见）;		
 2.除了&lt;fieldset&gt;元素之外，使用HTML标题（例如，&lt;h1&gt;、&lt;h2&gt;）和分段（如&lt;section&gt;）来构造一个复杂的表单也是一种常见的做法;     
 3.表单包含了&lt;section&gt;元素中包含的每个单独的功能部分，以及一个&lt;fieldset&gt;来包含单选按钮。     
 4.eg:构建一个表单结构		
 ![form1](form1.png)		
 	
 ```html
//HTML
<head>
  <meta charset="UTF-8">
  <link rel="stylesheet" href="html-basic.css">
</head>
<body>
<!-- 构建一个表单 -->
<form action="">
  <h1>Payment form</h1>
  <!-- 带*号的必填 <abbr title="required">*</abbr>  可以直接换成* -->
  <p>Required fields are followed by <strong><abbr title="required">*</abbr></strong>.</p>
  <section>
      <h2>Contact information</h2>
      <fieldset>
        <legend>Title</legend>
        <ul>
            <li>
              <label for="title_1">
                <input type="radio" id="title_1" name="title" value="M." >
                Mister
              </label>
            </li>
            <li>
              <label for="title_2">
                <input type="radio" id="title_2" name="title" value="Ms.">
                Miss
              </label>
            </li>
        </ul>
      </fieldset>
      <p>
        <label for="name">
          <span>Name: </span>
          <strong><abbr title="required">*</abbr></strong>
        </label>
        <input type="text" id="name" name="username">
      </p>
      <p>
        <label for="mail">
          <span>E-mail: </span>
          <strong><abbr title="required">*</abbr></strong>
        </label>
        <input type="email" id="mail" name="usermail">
      </p>
      <p>
        <label for="pwd">
          <span>Password: </span>
          <strong><abbr title="required">*</abbr></strong>
        </label>
        <!-- 密码输入框 默认密文 -->
        <input type="password" id="pwd" name="password">
      </p>
  </section>
  <section>
    <h2>Payment information</h2>
    <p>
      <label for="card">
        <span>Card type:</span>
      </label>
      <!-- 下拉选择框 -->
      <select id="card" name="usercard">
        <option value="visa">Visa</option>
        <option value="mc">Mastercard</option>
        <option value="amex">American Express</option>
      </select>
    </p>
    <p>
      <!-- label的for属性将label标签和表单小部件-input标签链接在一起，点击label也能使光标响应 -->
      <label for="number">
        <span>Card number:</span>
        <strong><abbr title="required">*</abbr></strong>
      </label>
        <input type="text" id="number" name="cardnumber">
    </p>
    <p>
      <label for="date">
        <span>Expiration date:</span>
        <strong><abbr title="required">*</abbr></strong>
        <em>formatted as mm/yy</em>
      </label>
      <input type="text" id="date" name="expiration">
    </p>
  </section>
  <!-- 添加按钮到表单底部,用于提交表单数据 -->
  <p> <button type="submit">Validate the payment</button> </p>
</form>
</body>
 ```		
 ```css
 //CSS
 
 h1 {
    margin-top: 0;
}

ul {
    margin: 0;
    padding: 0;
    list-style: none;
}

form {
    margin: 0 auto;
    width: 400px;
    padding: 1em;
    border: 1px solid #CCC;
    border-radius: 1em;
}

div+div {
    margin-top: 1em;
}

label span {
    display: inline-block;
    width: 120px;
    text-align: right;
}

input, textarea {
    font: 1em sans-serif;
    width: 250px;
    box-sizing: border-box;
    border: 1px solid #999;
}

input[type=checkbox], input[type=radio] {
    width: auto;
    border: none;
}

input:focus, textarea:focus {
    border-color: #000;
}

textarea {
    vertical-align: top;
    height: 5em;
    resize: vertical;
}

fieldset {
    width: 250px;
    box-sizing: border-box;
    margin-left: 136px;
    border: 1px solid #999;
}

button {
    margin: 20px 0 0 124px;
}

label {
  position: relative;
}

label em {
  position: absolute;
  right: 5px;
  top: 20px;
}
 ```
 
参考资料：	
1.[HTML表单](https://developer.mozilla.org/zh-CN/docs/learn/HTML)      
2.[构造表单](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Forms/How_to_structure_an_HTML_form)    
3.[fieldset](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/fieldset)     
4.[legend](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/legend)      
5.[checkbox-label](https://github.com/mdn/learning-area/blob/master/html/forms/html-form-structure/checkbox-label.html) 