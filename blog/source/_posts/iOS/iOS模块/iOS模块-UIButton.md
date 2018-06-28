---
title: iOS模块-UIButton
toc: true
date: 2018-06-28 09:27:59
tags: iOS模块
categories: iOS
---

一、UIButton简介<br>
![button2](button2.png)

<!-- more -->




二、UIButton的图文<br>
![button](button.png)

1.解析<br>
	
	UIEdgeInsets insets = {top, left, bottom, right};
    正数就是距相应的边的距离增加，负数就是距相应的距离减少
	
	button.imageView.image尺寸：
		button.currentImage.size.height
		button.currentImage.size.width
		
	button.titleLabel文本的尺寸：
		button.titleLabel.intrinsicContentSize.height
		button.titleLabel.intrinsicContentSize.width

	//1.图左文右 - 默认
    UIButton *button1 = [buttonArray objectAtIndex:0];
    [button1 setImageEdgeInsets:UIEdgeInsetsMake(0,0,0,0)];
    [button1 setTitleEdgeInsets:UIEdgeInsetsMake(0,0,0,0)];

	//2.图文居中
    UIButton *button2 = [buttonArray objectAtIndex:1];
    [button2 setImageEdgeInsets:UIEdgeInsetsMake(0,0,0,-button2.titleLabel.intrinsicContentSize.width)];
    [button2 setTitleEdgeInsets:UIEdgeInsetsMake(0,-button2.currentImage.size.width,0,0)];

	//3.图右文左
    UIButton *button3 = [buttonArray objectAtIndex:2];
    [button3 setImageEdgeInsets:UIEdgeInsetsMake(0,0,0,-button3.titleLabel.intrinsicContentSize.width * 2)];
    [button3 setTitleEdgeInsets:UIEdgeInsetsMake(0,-button3.currentImage.size.width * 2,0,0)];

	//4.图上文下
    UIButton *button4 = [buttonArray objectAtIndex:3];
    [button4 setImageEdgeInsets:UIEdgeInsetsMake(-button4.titleLabel.intrinsicContentSize.height,0,0,-button4.titleLabel.intrinsicContentSize.width)];
    [button4 setTitleEdgeInsets:UIEdgeInsetsMake(0,-button4.currentImage.size.width,-button4.currentImage.size.height,0)];

	//5.图文显示其一
    UIButton *button5 = [buttonArray objectAtIndex:4];

 	//6.图下 文上
    UIButton *button6 = [buttonArray objectAtIndex:5];
    [button6 setImageEdgeInsets:UIEdgeInsetsMake(0,0,-button6.titleLabel.intrinsicContentSize.height,-button6.titleLabel.intrinsicContentSize.width)];
    [button6 setTitleEdgeInsets:UIEdgeInsetsMake(-button6.currentImage.size.height,-button6.currentImage.size.width,0,0)]; 

三、UIButton点击事件<br>
![iOS上的响应者链](responder.png)<br>
1.阐述从点击屏幕上的UIButton后，到UIButton收到点击事件的过程

	runloop和响应链需要说的清楚。
	UIResponder、UIControl、UIView的关系。

	响应者链(responder chain)：
		当一个事件发生时，如果first responder不处理，事件就会继续往下传递，被下个responder接收，如果下个responder也不处理，又会被下下个rsponder接收...直到一个responder处理了事件或者没有responder。这些responder按照传递次序连接起来的链条就构成了响应者链。
		UIApplication 是一个响应者链的终点，它的nextResponder指向nil，整个响应者链结束。
	
		



参考资料:<br>
1.[iOS Events 和 Responder](https://www.zybuluo.com/MicroCai/note/66142)

