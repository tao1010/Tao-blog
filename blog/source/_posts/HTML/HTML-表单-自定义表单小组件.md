---
title: HTML-表单-自定义表单小组件
date: 2018-04-28 09:10:47
tags: html
categories: Web
---

一、设计、结构、语义		
1.控件的三个主要状态：正常状态（左侧）; 活动状态（中间）和打开状态（右侧） 
![custom-select](custom-select.png)	  

|状态|描述|
|---|---|
|正常|1.页面加载；<br>2.控件处于活动状态，但用户点击控件以外的任何位置；<br>3.控件是活动状态，但用户使用键盘将焦点移动到另一个小部件；<br>ps:在页面上移动焦点通常是按Tab键来完成，但这并不是标准(safari中是Option+Tab)|
|活动|1.用户点击;<br>2.用户按下tab让控件重新获得焦点;<br>3.控件呈现打开状态然后用户点击控件|
|打开|控件在非打开状态时被用户点击|
|值改变|1.控件在打开状态下用户点击一个选项；<br>2.控件在打开状态下用户按下键盘上方向键或者下方向键.|

2.自定义控件的选项变化
	
	当控件在打开状态时，选项将被突出显示;
	当鼠标悬停在某个选项上时，该选项将被突出显示，并且之前突出显示的选项将返回正常的状态;
3.定义语义话的HTML结构	
	
...







参考资料：	
1.[HTML表单](https://developer.mozilla.org/zh-CN/docs/learn/HTML)     
2.[自定义表单小部件](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Forms/How_to_build_custom_form_widgets)     
3.[用户可行性测试](https://en.wikipedia.org/wiki/Usability_testing)     