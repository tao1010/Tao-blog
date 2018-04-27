---
title: HTML-表单-表单数据校验
date: 2018-04-27 13:37:32
tags: html
categories:	Web
---

一、数据校验

	显示明确的错误消息。
	放宽输入格式。
	指出错误发生的位置（特别是在大型表单中）。
		
1.校验原因	

	希望以正确的格式获取正确的数据(手机号、邮箱等)；
	希望保护用户安全(密码设置规则等)；
	希望保护自己(确保不正确、不安全的数据不影响应用)；
2.不同类型的表单数据校验	
客户端验证：(用户体验更佳)
		
	表单数据被提交之前，浏览器端验证数据信息是否符合规则：
		JavaScript校验，完全自定义的实现方式；
		HTML5内置的校验，不需要JS，性能更好，但是无法自定义校验过程；
服务器端验证：
	
	浏览器提交数据并被服务器端程序接受之后，将数据写入数据库之前的阶段校验数据；
		如果数据有误，服务器端返回错误信息(发送错误的位置和原因)；
		如果数据正确，将数据持久化至数据库；
二、内置表单数据验证	
HTML5新功能：通过表单元素的校验属性实现：
	
	当元素通过校验时：
		该元素将可以通过CSS伪类:valid进行特殊的样式化；
		用户尝试提交表单，没有其他控制来阻止该操作；
	当元素未通过校验:
		该元素将可以通过 CSS 伪类 :invalid 进行列表的样式化；
		如果用户尝试提交表单，浏览器会展示出错误消息，并停止表单的提交;
1.input元素的验证约束 - required属性

```html
<div>
	<form >
	    <label for="name">Would you prefer a banana or cherry？</label>
	    <!--required属性，必须填写否则显示错误信息-->
	    <input type="text" id="name" placeholder="请选择你喜欢的水果" required>
	    <button type="submit">Submit</button>
	</form>
</div>
```
```css
<!--无效的样式-->
input:invalid {
  border: 2px dashed red;
}
<!--有效的样式-->
input:valid {
  border: 2px solid black;
}
```
![required1](required1.png)![required2](required2.png)     
2.input元素的验证约束 - pattern属性（正则表达式）    
正则表达式的工作原理:		
	
	a — 匹配一个字符a(不能匹配 b,  aa等等.)
	abc — 匹配 a, 其次 b, 最后 by c.
	a* — 匹配字符 a, 0个或者多个 (+ 代表至少匹配一个或者多个).
	[^a] — 匹配一个字符，但它不能是a.
	a|b — 匹配一个字符 a 或者 b.
	[abc] — 匹配一个字符，它可以是a,b或c.
	[^abc] — 匹配一个字符，但它不可以是a,b或c.
	[a-z] — 匹配字符范围 a-z且全部小写  (你可以使用 [A-z] 涵盖大小写, 或 [A-Z] 来限制必须大写).
	a.c — 匹配字符 a,中间匹配任意一个字符,最后匹配字符 c.
	a{5} — 匹配字符 a五次.
	a{5,7} — 匹配字符 a五到七次,不能多或者少.
	
	表达式中使用数字和其他字符:
	[ |-] — 匹配一个空格或者虚线.
	[0-9] — 匹配数字范围0~9.
	
	任意指定不同部分:
	[Ll].*k — 匹配一个大写L或者小写的l, 之后匹配一个或多个任意类型的字符, 最后匹配一个小写字母 k.
	[A-Z][A-z|-|']+ — 一个大写字母后面跟着匹配一个大小写字母或者中划线或者撇号. 这个可以用于校验英语会话中城市或城镇名, 但这需要首字母以大写开头,不包括其他字符(你可以添加额外的表达式来做到).
	[0-9]{3}[ |-][0-9]{3}[ |-][0-9]{4} — 简单的匹配一个美国内的电话号码 — 三个数字 0-9, 后面跟着一个空格或者中划线, 之后匹配三个数字 0-9, 再跟着一个空格或者中划线, 最后跟着四个数字 0-9. 但实际情况可能更加复杂,因为有些人会给号码加上括号什么的,这里的表达式只是用来做一个简单的演示.

```html
<form >
    <label for="name">Would you prefer a banana or cherry？</label>
    <input type="text" id="name" placeholder="请选择你喜欢的水果" required pattern="banana|cherry">
    <button type="submit">Submit</button>
</form>
```
有些input元素不需要pattern属性进行验证，如：
	
	指定input的type=email就在使用匹配邮件格式的正则表达式去验证；
	指定input的type=url
	指定input的type=email multiple 验证输入中是否有“,”
textarea元素不支持pattern属性。

3.input元素的验证约束 - 强制条目长度		
所有文本框 (&lt;input&gt; 或 &lt;textarea&gt;) 可以强制使用minlength 和 maxlength 属性. 如果值小于该字段 minlength 的值或大于 maxlength 值则无效. 浏览器通常不会组织用户在文本字段中输入比预期更长的值，但是可以使用这种细粒度的控件来强制。

在数字条目中 (i.e. &lt;input type="number"&gt;), 该 min 和 max 属性也能强制验证长度. 如何条目中的长度小于min 属性提供的值或大于 max 属性的值,该条目则无效. 

```html
<div>
    <form >
      <div>
        <label for="name">Would you prefer a banana or cherry？</label>
        <!-- pattern="banana|cherry" -->
        <input type="text" id="name" placeholder="请选择你喜欢的水果" required minlength="1" maxlength="6">
       </div>
       <div>
         <label for="name">How many would you like?</label>
         <!-- 也可以设置step属性 -->
         <input type="number" id="number" name="amount" value="1" min="1" max="10">
       </div>
       <button type="submit">Submit</button>
    </form>
</div>
```
4.自定义错误信息

```html
<div>
  <span>完整例子</span>
  <form >
    <p>
      <fieldset>
        <legend>Title <abbr title="This field is mandatory">*</abbr></legend>
        <input type="radio" name="title" id="r1" value="Mr" required><label for="r1">Mr.</label>
        <input type="radio" name="title" id="r2" value="Ms" required><label for="r2">Ms.</label>
      </fieldset>
    </p>
    <p>
      <label for="n1">How old are you?</label>
      <input type="number" name="age" id="n1" max="120" min="12" step="1" pattern="\d+">
    </p>
    <p>
      <label for="t1">What's your favite fruit? <abbr title="This field is mandatory">*</abbr></label>
      <input type="text" id="t1" name="fruit" list="l1" required pattern="[Bb]anana|[Cc]herry|[Aa]pple|[Ss]trawberry|[Ll]emon|[Oo]range">
      <datalist id="l1">
        <option >Banana</option>
        <option >Cherry</option>
        <option >Apple</option>
        <option >Strawberry</option>
        <option >Lemon</option>
        <option >Orange</option>
      </datalist>
    </p>
    <p>
      <label for="t2">What's your email?</label>
      <input type="email" name="email" id="t2">
    </p>
    <p>
      <label for="t3">Leave a short message</label>
      <textarea name="msg" id="t3" maxlength="140" rows="5"></textarea>
    </p>
    <p>
      <button>Submit</button>
    </p>
  </form>
</div>
```
```css
body {
  font: 1em sans-serif;
  padding: 0;
  margin : 0;
}

form {
  max-width: 200px;
  margin: 0;
  padding: 0 5px;
}

p > label {
  display: block;
}

input[type=text],
input[type=email],
input[type=number],
textarea,
fieldset {
/* required to properly style form 
   elements on WebKit based browsers */
  -webkit-appearance: none;
  
  width : 100%;
  border: 1px solid #333;
  margin: 0;
  
  font-family: inherit;
  font-size: 90%;
  
  -moz-box-sizing: border-box;
  box-sizing: border-box;
}
<!--无效的输入-->
input:invalid {
  box-shadow: 0 0 5px 1px red;
}
<!--有效的输入-->
input:focus:invalid {
  outline: none;
}
```
三、JS校验表单			
想控制原生错误信息的外观和感觉，或不想处理不支持HTML内置表单验证的浏览器，必须使用JS。    
1.HTML5用于校验约束的API	
用于校验的API及属性

|属性|描述|
|---|---|
|validationMessage|	一个本地化消息，描述元素不满足验证条件时（如果有的话）的文本信息。如果元素无需验证（willValidate 为 false），或元素的值满足验证条件时，为空字符串。|
|validity|	一个 ValidityState 对象，描述元素的验证状态。|
|validity.customError|	如果元素设置了自定义错误，返回 true ；否则返回false。|
|validity.patternMismatch|	如果元素的值不匹配所设置的正则表达式，返回 true，否则返回 false。当此属性为 true 时，元素将命中  :invalid CSS 伪类。|
|validity.rangeOverflow |	如果元素的值高于所设置的最大值，返回 true，否则返回 false。当此属性为 true 时，元素将命中  :invalid CSS 伪类。|
|validity.rangeUnderflow|	如果元素的值低于所设置的最小值，返回 true，否则返回 false。当此属性为 true 时，元素将命中  :invalid CSS 伪类。|
|validity.stepMismatch|	如果元素的值不符合 step 属性的规则，返回 true，否则返回 false。当此属性为 true 时，元素将命中  :invalid CSS 伪类。|
|validity.tooLong|	如果元素的值超过所设置的最大长度，返回 true，否则返回 false。当此属性为 true 时，元素将命中  :invalid CSS 伪类。|
|validity.typeMismatch|	如果元素的值出现语法错误，返回 true，否则返回 false。当此属性为 true 时，元素将命中  :invalid CSS 伪类。
|validity.valid|	如果元素的值不存在验证问题，返回 true，否则返回 false。当此属性为 true 时，元素将命中  :valid CSS 伪类，否则命中 :invalid CSS 伪类。
|validity.valueMissing	|如果元素设置了 required 属性且值为空，返回 true，否则返回 false。当此属性为 true 时，元素将命中  :invalid CSS 伪类。|
|willValidate	|如果元素在表单提交时将被验证，返回 true，否则返回 false。|

校验约束API的方法	
	
|方法|描述|
|---|---|
|checkValidity()|如果元素的值不存在验证问题，返回 true，否则返回 false。如果元素验证失败，此方法会触发invalid 事件。|
|setCustomValidity(message)|为元素添加一个自定义的错误消息；如果设置了自定义错误消息，则该元素被认为是无效的，并显示指定的错误。这允许你使用 JavaScript 代码来建立验证失败，而不是用标准约束验证 API 所提供的。在报告问题时，将向用户显示自定义消息。如果参数为空，则清空自定义错误|

2.例子

```html
<div>
  <span>使用校验约束API例子</span>
  <!--使用 novalidate 属性关闭浏览器的自动验证；这允许我们使用脚本控制表单验证。这并不禁止支持约束验证 API 的应用和以下 CSS 伪类：:valid、:invalid、:in-range 、:out-of-range 。这意味着，即使浏览器在发送数据之前没有自动检查表单的有效性，您仍然可以自己做，并相应地设置表单的样式。-->
  <form novalidate>
    <p>
      <label for="mail">
        <span>Please enter an email address:</span>
        <input type="email" name="mail" id="mail">
        <!--aria-live 属性确保我们的自定义错误信息将呈现给所有人，包括使用屏幕阅读器等辅助技术的人。-->
        <span class="error" aria-live="polite"></span>
      </label>
    </p>
    <button type="submit">Submit</button>
  </form>
</div>
```
```css
/* 仅为了使示例更好看 */
body {
  font: 1em sans-serif;
  padding: 0;
  margin : 0;
}

form {
  max-width: 200px;
}

p * {
  display: block;
}

input[type=email]{
  -webkit-appearance: none;

  width: 100%;
  border: 1px solid #333;
  margin: 0;

  font-family: inherit;
  font-size: 90%;

  -moz-box-sizing: border-box;
  box-sizing: border-box;
}

/* 验证失败的元素样式 */
input:invalid{
  border-color: #900;
  background-color: #FDD;
}

input:focus:invalid {
  outline: none;
}

/* 错误消息的样式 */
.error {
  width  : 100%;
  padding: 0;
 
  font-size: 80%;
  color: white;
  background-color: #900;
  border-radius: 0 0 5px 5px;
 
  -moz-box-sizing: border-box;
  box-sizing: border-box;
}

.error.active {
  padding: 0.3em;
}
```
```js
// 有许多方式可以获取 DOM 节点；在此我们获取表单本身和
// email 输入框，以及我们将放置错误信息的 span 元素。

var form  = document.getElementsByTagName('form')[0];
var email = document.getElementById('mail');
var error = document.querySelector('.error');

email.addEventListener("input", function (event) {
  // 当用户输入信息时，验证 email 字段
  if (email.validity.valid) {
    // 如果验证通过，清除已显示的错误消息
    error.innerHTML = ""; // 重置消息的内容
    error.className = "error"; // 重置消息的显示状态
  }
}, false);
form.addEventListener("submit", function (event) {
  // 当用户提交表单时，验证 email 字段
  if (!email.validity.valid) {
    
    // 如果验证失败，显示一个自定义错误
    error.innerHTML = "I expect an e-mail, darling!";
    error.className = "error active";
    // 还需要阻止表单提交事件，以取消数据传送
    event.preventDefault();
  }
}, false);
```
3.不使用内建API时的表单验证		
	
	怎样验证表单？
	验证失败怎么处理？
	如何帮助用户纠正无效数据？
eg:

```html
<form>
  <p>
    <label for="mail">
        <span>Please enter an email address:</span>
        <input type="text" class="mail" id="mail" name="mail">
        <span class="error" aria-live="polite"></span>
    </label>
  <p>
  <!-- Some legacy browsers need to have the `type` attribute
       explicitly set to `submit` on the `button`element -->
  <button type="submit">Submit</button>
</form>
```
```css
//将 :invalid 伪类变成真实的类，并避免使用不适用于 Internet Explorer 6 的属性选择器
/* 仅为了使示例更好看 */
body {
  font: 1em sans-serif;
  padding: 0;
  margin : 0;
}

form {
  max-width: 200px;
}

p * {
  display: block;
}

input.mail {
  -webkit-appearance: none;

  width: 100%;
  border: 1px solid #333;
  margin: 0;

  font-family: inherit;
  font-size: 90%;

  -moz-box-sizing: border-box;
  box-sizing: border-box;
}

/* 验证失败的元素样式 */
input.invalid{
  border-color: #900;
  background-color: #FDD;
}

input:focus.invalid {
  outline: none;
}

/* 错误消息的样式 */
.error {
  width  : 100%;
  padding: 0;
 
  font-size: 80%;
  color: white;
  background-color: #900;
  border-radius: 0 0 5px 5px;
 
  -moz-box-sizing: border-box;
  box-sizing: border-box;
}

.error.active {
  padding: 0.3em;
}
```
```js
// 使用旧版浏览器选择 DOM 节点的方法较少
var form  = document.getElementsByTagName('form')[0];
var email = document.getElementById('mail');

// 以下是在 DOM 中访问下一个兄弟元素的技巧
// 这比较危险，很容易引起无限循环
// 在现代浏览器中，应该使用 element.nextElementSibling
var error = email;
while ((error = error.nextSibling).nodeType != 1);

// 按照 HTML5 规范
var emailRegExp = /^[a-zA-Z0-9.!#$%&'*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$/;

// 许多旧版浏览器不支持 addEventListener 方法
// 这只是其中一种简单的处理方法
function addEvent(element, event, callback) {
  var previousEventCallBack = element["on"+event];
  element["on"+event] = function (e) {
    var output = callback(e);
    
    // 返回 `false` 来停止回调链，并中断事件的执行
    if (output === false) return false;

    if (typeof previousEventCallBack === 'function') {
      output = previousEventCallBack(e);
      if(output === false) return false;
    }
  }
};

// 现在我们可以重构字段的验证约束了
// 由于不使用 CSS 伪类, 我们必须明确地设置 valid 或 invalid 类到 email 字段上
addEvent(window, "load", function () {
  // 在这里验证字段是否为空（请记住，该字段不是必需的）
  // 如果非空，检查它的内容格式是不是合格的 e-mail 地址
  var test = email.value.length === 0 || emailRegExp.test(email.value);
 
  email.className = test ? "valid" : "invalid";
});

// 处理用户输入事件
addEvent(email, "input", function () {
  var test = email.value.length === 0 || emailRegExp.test(email.value);
  if (test) {
    email.className = "valid";
    error.innerHTML = "";
    error.className = "error";
  } else {
    email.className = "invalid";
  }
});

// 处理表单提交事件
addEvent(form, "submit", function () {
  var test = email.value.length === 0 || emailRegExp.test(email.value);
 
  if (!test) {
    email.className = "invalid";
    error.innerHTML = "I expect an e-mail, darling!";
    error.className = "error active";

    // 某些旧版浏览器不支持 event.preventDefault() 方法
    return false;
  } else {
    email.className = "valid";
    error.innerHTML = "";
    error.className = "error";
  }
});
```
许多库可用于执行表单验证:
	
	独立的库（原生JS实现）Validate.js
	jQuery插件（Validation、Valid8）
4.远程校验			
当用户输入的数据与存储在应用程序服务器端的附加数据绑定时，这种验证是必要的。 

eg:
	
	一个用例就是注册表单，在这里你需要一个用户名。 为了避免重复，执行一个 AJAX 请求来检查用户名的可用性，而不是要求用户发送数据，然后发送带有错误的表单。

执行这样的验证需要采取一些预防措施：

	它要求公开 API 和一些数据；您需要确保它不是敏感数据。
	网络滞后需要执行异步验证。这需要一些用户界面的工作，以确保如果验证不正确执行，用户不会被阻止。


参考资料：	
1.[表单数据校验](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Forms/Data_form_validation)		
2.[表单](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Forms)		
