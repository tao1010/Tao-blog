---
title: HTML-表单-原生表单小部件
date: 2018-04-26 10:21:50
tags: html
categories: Web
---

原生表单部件(组件) - 浏览器内置表单		
一、通用属性		

|属性名称|默认值|描述|
|---|---|---|
|autofocus|false|允许您指定当页面加载时元素应该自动具有输入焦点，除非用户覆盖它，例如通过键入不同的控件。文档中只有一个与表单相关的元素可以指定这个属性|
|disabled|false|默认表示用户不能与元素交互。如果没有指定这个属性，元素将从包含的元素继承它的设置，例如&lt;fieldset&gt;;如果没有包含disabled属性集的元素，那么就启用了元素|
|form||小部件与之相关联的表单元素。属性值必需是同个文档中的&lt;form&gt; 属性的 id属性。理论上，它允许您在&lt;form&gt;元素之外设置一个表单小部件。然而，在实践中，没有任何支持该特性的浏览器。|
|name||元素的名称,用于表单数据提交的|
|value||元素的初始值|
二、文本输入域 - &lt;input&gt;		
1.HTML表单文本字段是简单的纯文本输入控件。不能使用它们执行富文本编辑(粗体、斜体等)。将遇到的所有富文本编辑器（rich text editors）都有使用HTML、CSS和JavaScript创建的自定义小部件。     
2.通用规范：		

	可以被标记为readonly,甚至disabled(输入值永远不会与表单数据的其余部分一起发送);
	可以有一个placeholder,用来简述输入框的目的;
	可以被限制在size(框的物理尺寸)和长度(输入的最大字符数)；
	可以从拼写检查中获益(浏览器支持才行)。
3.单行文本域 - &lt;input&gt;		
元素可以是任何东西，通过简单的设置type属性，可以用于创建大多数类型的表单小部件(单行文本字段、没有文本输入的控件、时间和日期控件、按钮),多行输入时用&lt;textarea&gt;元素. 
		
```html
//HTML
<h1>原生小部件</h1>
<div>
  <!-- 单行文本域： 设置type = text即可默认也是text -->
  <label for="name">姓名:</label>
  <input type="text" id="name" placeholder="请输入姓名">
</div>
<div>
  <!-- 地址域：用户必须在域中输入有效的邮箱地址，否则浏览器提交表单会报错, multiple属性可以让用户输入多个邮箱，以“,”分开 -->
  <label for="email">邮箱:</label>
  <input type="email" id="email" placeholder="请输入邮箱">
    
  <label for="emails">多个邮箱:</label>
  <input type="email" id="emails" placeholder="请输入邮箱(支持多个)" multiple>
</div>
<div>
  <!-- 密码域：用户看到的是密文，但是提交表单数据的仍是以明文发送的，为了安全需要加密之类的处理提交数据 -->
  <label for="password">密码:</label>
  <input type="password" name="pwd" id="password" placeholder="请输入密码">
</div>
<div>
  <!-- 搜索域：文本域和搜索域主要区别是浏览器的样式，搜索字段以圆角和/或给定的一个“x”来清除输入的值，其值可以自动保存到同一站点的多个页面上自动完成 -->
  <label for="search">搜索:</label>
  <input type="search" name="sea" id="search">
</div>
<div>
  <!-- 电话号码域:不会限制用户输入的内容(字母等都可以) -->
  <label for="tel">电话:</label>
  <input type="tel" name="telphone" id="tel">
</div>
<div>
  <!-- URL域：为输入的字段添加了特殊的验证约束，若输入无效的url，浏览器会报错 -->
  <label for="url">URL:</label>
  <input type="url" name="urlname" id="url">
</div>
<div>
  <!--日期域： 浏览器不支持原生日期选择器 -->
  <label for="today">日期:</label>
  <input type="date" name="date" id="today">
</div>

<div>
  <!-- 按钮域 -->
  <label for="button"></label>
  <input type="button" value="input的按钮">
</div>
<div>
  <!-- 多行文本域：见下文->
  <label for="morerow">多行输入:</label>
  <textarea name="text" id="morerow" cols="30" rows="10"></textarea>
</div>

```
![element](element.png)			
4.多行文本域 - &lt;textarea&gt;			
和单行的区别:
	
	<input>    :元素是一个空元素，不能包含任何子元素；
				  定义一个默认值时，必须用value属性；
	
	<textarea> :元素是一个常规元素，可以包含文本内容的子元素；
		   		  允许用户输入包含硬换行符(即按回车)的文本；
		   		  定义一个默认值，只需将默认文本放在 <textarea ...>默认文本</textarea>；
		   		  只接受文本内容，即是任何HTML内容放入<textarea>中都将以纯文本内容呈现。
属性：
	
	cols：文本控件的可见宽度，平均字符宽度 默认20
    rows：控制的可见文本行数
    wrap：表示控件是如何包装文本的 默认soft  可切换 hard | soft
tips:右下角有个调整大小的区域，需要在CSS中设置resize = none来关闭(chorme的浏览器不需要设置)			
三、下拉窗口小部件		
1.HTML有两类下拉内容:select box和autocomplete box，它们交互是相同的，一旦控件被激活，浏览器会显示用户可以选择的值列表。		
2.选择框		

```html
<h2>下拉窗口小部件</h2>

<!-- 如果在option中设置了value属性，当提交表单的时候，该属性的值就会被发送，若忽略value值，则使用option元素的内容作为选择框的值 -->
<div>
  <label for="simple">单项选择框:</label>
  <!-- 选择框：通过select元素创建，包含一个或多个option子元素，设置子元素的selected 可以将其设置为默认选项 -->
  <select name="simple" id="simple">
    <option>Banana</option>
    <option selected>Cherry</option>
    <option value="hello">Lemon</option>
  </select>
</div>

<div>
    <label for="groups">分组单选框:</label>
    <!-- 分组:将option元素嵌套在optgroup元素中，嵌套在select元素中; optgroup的label属性显示在值前，它不可选-->
    <select name="groups" id="groups">
      <optgroup label="Fruits">
        <option>Banana</option>
        <option value="2" selected>Cherry</option>
        <option value="3">Lemon</option>
      </optgroup>
      <optgroup label="Vegetables">
        <option value="1">Carrot</option>
        <option value="2">Egglant</option>
        <option value="3">Potato</option>
      </optgroup>

    </select>
</div>

<div>
  <label for="mutiple">多选选择框:</label>
  <!-- 多选选择框:在select元素上加上multiple属性即可，允许设置多个默认值，cmd|ctrl +鼠标 选择多个值 -->
  <select name="multi" id="muti" multiple>
    <option value="1" selected>Banana</option>
    <option value="2" selected>Cherry</option>
    <option value="3">Lemon</option>

  </select>
</div>
```
![selected1](selected1.png)  ![selected2](selected2.png)      
3.自动补全输入框			

```html
<div>
  <label for="name">自动补全输入框:</label>
  <!-- 自动补全输入框:使用datalist元素来为表达小部件提供建议、自动完成的值，并使用option子元素来指定要显示的值，
  使用list属性将数据列表绑定到一个文本域-input，绑定完成，选项用于自动完成用户输入的文本，并以下拉向用户展示，在输入框输入可匹配的内容。 -->
  <label for="myFruit">What's your faavorite fruit?</label>
  <input type="text" name="myFruit" id="myFruit" list="mySuggestion">
  <datalist id="mySuggestion">
    <option value="1">Apple</option>
    <option >Banana</option>
    <option >BlackBerry</option>
    <option value="2">Lemon</option>
    <option value="3">Lychee</option>
    <option value="4">Peach</option>
    <option value="5">Pear</option>
    
  </datalist>
</div>
```
![auto1](auto1.png)		
tips:
	
	根据HTML规范，list 属性和<datalist>元素元素可以用于任何需要用户输入的小部件;
	但是，除了文本(例如颜色或日期)，它应该如何工作还不清楚，不同的浏览器在不同的情况下会有不同的表现。正因为如此，除了文本字段以外，要小心使用这个特性。
数据列表支持和兼容		

	<datalist>元素是HTML表单的最新补充，它在10以下的IE版本中不受支持，Safari在写作时仍然不支持它。
兼容	
	
	支持<datalist>元素的浏览器将忽略所有不是<option>元素的元素，并按照预期工作。
	不支持<datalist>元素的浏览器将显示标签和选择框。
	还有其他方法可以处理<datalist> 元素的不足，但这是最简单的(其他方法往往需要JavaScript)

```html
<div>
  <label for="all">数据列表浏览器版本兼容:</label>
  <label for="mtFruit">What's is your favorite fruit?(With fallback)</label>
  <input type="text" name="fruit" id="myFruit" list="fruitlist">
  <datalist id="fruitlist">
    <label for="suggestion">or pick a fruit</label>
    <select name="altFruit" id="suggestion">
        <option >Apple</option>
        <option >Banana</option>
        <option >BlackBerry</option>
        <option >Lemon</option>
        <option >Lychee</option>
        <option >Peach</option>
        <option >Pear</option>
    </select>
  </datalist>
</div>
```
四、可选中项		
可选中项是状态可以通过单击它们来改变小部件，包含:复选框和单选按钮，使用checked属性，表示是否在默认情况下进行检查。在发送表单数据时，只有勾选的内容的值才被发送。    
1.复选框

```html
<div>
  <label for="checkbox">复选框</label>
  <input type="checkbox" name="carrots" id="carrots" checked value="carrots">
</div>
```
2.单选按钮

```html
<div>
  <label for="radio">单选按钮</label>
  <input type="radio" name="meal" id="soup" checked>
</div>
```

3.eg:	

```html
<div>
  <label for="name">复选框和单选框</label>
  <fieldset>
    <legend>What is your favorite meal?</legend>
    <ul>
      <li>
        <label for="soup">Soup</label>
        <input type="radio" checked id="soup" name="meal" value="soup">;
      </li>
      <li>
        <label for="curry">Curry</label>
        <input type="radio" id="curry" name="meal" value="curry">
      </li>
      <li>
        <label for="pizza">Pizza</label>
        <input type="radio" name="meal" id="pizza" value="pizza">
      </li>
    </ul>
  </fieldset>
</div>
```
![radio](radio.png)			
五、按钮		
1.在HTML表单中，有三种按钮:
	
	Submit:将表单数据发送到服务器;
	Reset:将所有表单小部件重新设置为它们的默认值;
	Anonymous:没有自动生效的按钮，但是可以使用JS代码定制，如果省略type属性，那么就是默认值;
2.使用&lt;button&gt;元素或者&lt;input&gt;元素创建一个按钮，type属性的值指定显示什么类型的按钮。	

```html
<h2>按钮</h2>
<div>
  <button type="submit">This a <br><strong>submit button</strong></button>
  <input type="submit" value="This is input creat button">
</div>

<div>
  <button type="reset">This a <br> <strong>reset button</strong></button>
  <input type="reset" value="This is input creat button">
</div>

<div>

  <button type="button">This an <br><strong>anonymous button</strong></button>
  <input type="button" value="This is input creat button">
</div>
```	
![button](button.png)		
button和input的区别:
	
	<button>元素允许您在它们的标签中使用HTML内容，这些内容被插入到打开和关闭<button> 标签中。另一方面，<input>元素是空元素;它们的标签被插入到value属性中，因此只接受纯文本内容;
	使用<button>元素，可以有一个不同于按钮标签的值(通过将其设置为value属性)。这在IE 8之前的版本中是不可靠的;
	从技术上讲，使用<button>元素或<input>元素定义的按钮几乎没有区别。唯一值得注意的区别是按钮本身的标签。在<input>元素中，标签只能是字符数据，而在<button>元素中，标签可以是HTML，因此可以相应地进行样式化.
六、高级表单部件		
1.数字		
	
	input type = number 这是一个只允许浮点数的输入框，
	并通过提供一些按钮来增加或减小小部件的值
    设置min和max 属性来约束值，设置step属性来指定增加和减少按钮更改小部件的值的数量。
	IE10一下不支持number输入.
	  
```html
<div>
    <label for="name">数字:</label>
    <input type="number" name="age" id="age" min="1" max="100" step="2">
</div>
```

2.滑块			

	使用滑块。从视觉上讲，滑块比文本字段更不准确，因此它们被用来选择一个确切值并不重要的数字;
	input type = range 正确配置滑块是很重要的；强烈建议您设置min、max和step属性;
	不提供任何形式的视觉反馈，以了解当前的值是什么。需要使用JavaScript来添加这一点
	
```html
<div>
  <label for="name">滑块:</label>
  <input type="range" name="beans" id="beans" min="0" max="50" step="10">
  <span class="beancount"></span>
</div>
```
```js
var beans = document.querySelector('#beans');
var count = document.querySelector('.beancount');
count.textContent = beans.value;

beans.oninput = function() {
  count.textContent = beans.value;
}
```
3.日期时间选择器	

	所有日期和时间控制都可以使用min和max属性来约束；
	警告——日期和时间窗口小部件仍然很不受支持。目前，Chrome、Edge和Opera都支持它们，但IE浏览器没有支持，Firefox和Safari对这些都没有太大的支持。
	
```html
<div>
  <label for="name">日期 - 本地时间:</label>
  <input type="datetime-local" name="datetime" id="datetime">

</div>

<div>
    <label for="name">日期 - 月:</label>
    <input type="month" name="month" id="month">
</div>
<div>
    <label for="name">日期 - 时间:</label>
    <input type="time" name="time" id="time">
  
</div>

<div>
    <label for="name">日期 - 星期:</label>
    <input type="week" name="week" id="week">

</div>
<div>
    <label for="myDate">When are you available this summer?</label>
    <input type="date" name="myDate" min="2013-06-01" max="2013-08-31" id="myDate">
</div>
```	
![date](date.png)		
4.拾色器		
input type = color RGB值(十进制或十六进制)、HSL值、关键字等等, 颜色小部件允许用户在文本和可视的方式中选择颜色.

```html
<div>
  <label for="name">拾色器:</label>
  <input type="color" name="color" id="color">
</div>
```
颜色小部件支持它目前不是很好。IE中没有支持，Safari目前也不支持它。其他主要的浏览器都支持它.			
七、其他小部件		
1.文件选择器	
input type = file accept属性约束被接受文件类型，添加multiple属性来实现选择多文件.

```html
<div>
  <label for="file">文件选择器:</label>
  <input type="file" name="file" id="file" accept="pdf" multiple>
</div>
```	
2.隐藏内容		
input type = hidden用户不可见表单数据

```html
<div>
  <label for="name">隐藏内容:</label>
  <input type="hidden" name="timestamp" id="timestamp" value="1231941431123">
</div>
```
4.图像按钮		
input type = image 图像按钮控件是一个与&lt;img&gt;元素完全相同的元素，除了当用户点击它时，它的行为就像一个提交按钮.
	
	如果使用图像按钮来提交表单，这个小部件不会提交它的值；相反，在图像上单击的X和Y坐标是被提交的(坐标是相对于图像的，这意味着图像的左上角表示坐标0，0)，坐标被发送为两个键/值对：
		X值键是name属性的值，后面是字符串“.x”。
		Y值键是name属性的值，后面是字符串“.y”。

```html
<div>
  <label for="name">图像按钮:</label>
  <input type="image" src="111.png" alt="Click me!" width="80" height="30">
</div>
```
5.仪表和进度条		
仪表和进度条是数值的可视化表示。		

	仪表：meter(不支持元素的浏览器回退，辅助技术对其发声)元素创建，表示一个固定值，值由min和max决定，并以条形的方式显示。
		 low 和 high 值范围划分为三个部分：
			该范围的较低部分是在min和low值(包括那些值)之间。
			该范围的中间部分是在low和high值之间(不包括那些值)。
			该范围的较高部分是在high和max值(包括那些值)之间。
		optimum值定义了<meter>元素的最优值。在与optimum和high值的联合中，它定义了该范围的哪个部分是优先的：
			如果optimum)值在较低的范围内，则较低的范围被认为是首选项，中等范围被认为是平均值，而较高的范围被认为是最坏的部分。
			如果optimum值在该范围的中等部分，则较低的范围被认为是一个平均值，中等范围被认为是优先的部分，而较高的范围也被认为是平均值。
			如果optimum值在较高的范围内，则较低的范围被认为是最坏的部分，中等范围被认为是一般的部分，较高的范围被认为是优先的部分。
	所有实现<meter>元素的浏览器都使用这些值来改变米尺的颜色。

		如果当前值位于该范围的优先部分，则该条是绿色的。
		如果当前值位于该范围的平均部分，则该条是黄色的。
		如果当前值处于最糟糕的范围，则该条是红色的。
		
	进度条： progress(不支持元素的浏览器回退，辅助技术对其发声)元素创建，一个进度条表示一个值(它会随时间的变化而变化到最大的值，由max属性指定)。

```html
<h1>自定义小部件</h1>

<div>
  <label for="file">文件选择器:</label>
  <input type="file" name="file" id="file" accept="pdf" multiple>
</div>

<div>
  <label for="name">隐藏内容:</label>
  <input type="hidden" name="timestamp" id="timestamp" value="1231941431123">
</div>

<div>
  <label for="name">图像按钮:</label>
  <input type="image" src="111.png" alt="Click me!" width="80" height="30">
</div>

<div>
  <label for="name">进度条:</label>
  <progress max="100" value="75">75/100</progress>
</div>

 <div>
   <label for="name">仪表:</label>
   <!-- 当它开始满时，它会变成红色 -->
   <meter min="0" max="100" value="75" low="33" high="66" optimum="50">75</meter>
 </div>
```
![other](other.png)		

参考资料：	
1.[HTML表单](https://developer.mozilla.org/zh-CN/docs/learn/HTML)     
2.[表单小组件](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Forms/The_native_form_widgets)     
3.[发送和检索表单数据](https://developer.mozilla.org/en-US/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data)     
