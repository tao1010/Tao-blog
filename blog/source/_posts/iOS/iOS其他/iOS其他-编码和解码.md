---
title: iOS其他-编码和解码
toc: true
date: 2018-07-04 14:46:31
tags: iOS其他
categories: iOS
---

1.OC中对中文编码

<!-- more -->

	NSString *city = @"成都";
	//正确编码为: %e6%88%90%e9%83%bd
	
	最新方法：
	方法一：
		NSString *charactersToEscape = @"?!@#$^&%*+,:;='\"`<>()[]{}/\\| ";
    	NSCharacterSet *allowedCharacters = [[NSCharacterSet characterSetWithCharactersInString:charactersToEscape] invertedSet];
    	
	方法二：
		NSCharacterSet *allowedCharacters = [NSCharacterSet URLQueryAllowedCharacterSet]
		
	NSString *city_encode = [city stringByAddingPercentEncodingWithAllowedCharacters:allowedCharacters];
	已废弃方法：
		NSString *city_encode = [city stringByAddingPercentEscapesUsingEncoding:NSUTF8StringEncoding];
对中文、特殊符号 & % 和空格 都必须转译才能正确访问，一般需要转译的字符:@"<font color='red'>?!@#$^&%*+,:;='\"`<>()[]{}/\\| </font>"	
2.Unicode和汉字互转

	iOS9-
	
	//unicode转中文
	NSString* strA = [@"%E4%B8%AD%E5%9B%BD"stringByReplacingPercentEscapesUsingEncoding:NSUTF8StringEncoding];
	//中文转unicode
	NSString *strB = [@"中国"stringByAddingPercentEscapesUsingEncoding:NSUTF8StringEncoding];

	ios9+
	
	//中文转unicode
	urlStr = [@"url地址" stringByAddingPercentEncodingWithAllowedCharacters:[NSCharacterSet URLQueryAllowedCharacterSet]];
	//unicode转中文
	urlStr = [@"url地址" stringByRemovingPercentEncoding];

3.区别stringByAdding... 和stringByReplacing...

	...Adding...
		将Unicode字符转换成有百分号的形式;
		对中文和一些特殊字符进行编码;
	...Adding...
		将百分号形式转换成Unicode形式

参考资料:<br>
1.[站长工具 - URL编码](http://tool.chinaz.com/tools/urlencode.aspx)<br>
2.[stringByAdding... 和stringByReplacing...](https://blog.csdn.net/u011774517/article/details/51295103)