---
title: Hexo在GitHub上的异常处理
date: 2018-03-23 15:45:18
tags: hexo
categories: 博客

---

一、意外的标记异常

1.异常内容：
	
	xxx:blog xxxx$ hexo g
	
	INFO  Start processing
	FATAL Something's wrong. Maybe you can find the solution here: http://hexo.io/docs/troubleshooting.html
	Template render error: unexpected token: }}
	    at Object._prettifyError (/Users/xxx/xxx/xxx/blog/node_modules/nunjucks/src/lib.js:35:11)
	    at Template.render (/Users/xxx/xxx/xxx/blog/node_modules/nunjucks/src/environment.js:526:21)
	  ...
2.异常原因：

	这类异常一般是文章中使用了大括号 { } 这个特殊字符,且没有转义导致编译不通过
3.解决方案：(参考Markdown语法)
	
	将 { } 的大括号通过&#123; &#125; 进行转换	
	
二、模版渲染错误异常

1.异常内容：

	Template render error: parseSignature: expected comma after expression
        at Error.exports.TemplateError (User//xxx/xxx/xxx/blog/node_modules/hexo/node_modules/nunjucks/src/lib.js:51:19)
        at Object.extend.fail (/xxx/xxx/xxx/blog/node_modules/hexo/node_modules/nunjucks/src/parser.js:64:15)
        at Object.extend.parseSignature (/xxx/xxx/xxx/blog/node_modules/hexo/node_modules/nunjucks/src/parser.js:1077:22)
        ...
2.异常原因:

	这类异常一般是文章中使用了某个特殊字符, 解析时讲表达式中的内容按函数处理了,特殊字符没有转义导致编译不通过
	
3.解决方案：

	将特殊字符通过转义进行转换;(最好的方案是避免写特殊字符)

三、无法执行now函数异常

1.异常内容:

	  FATAL (unknown path) [Line 7, Column 533]
	  Error: Unable to call `now`, which is undefined or falsey
	Template render error: (unknown path) [Line 7, Column 533]
	  Error: Unable to call `now`, which is undefined or falsey
	    at Object.exports.prettifyError (/Users/xxx/xxx/xxx/blog/node_modules/nunjucks/src/lib.js:34:15)
	    at /Users/xxx/xxx/xxx/blog/node_modules/nunjucks/src/environment.js:485:31
	    at root [as rootRenderFunc] (eval at <anonymous> (/Users/xxx/xxx/xxx/blog/node_modules/nunjucks/src/environment.js:564:24), <anonymous>:20:3)
	    ...
2.异常原因:

	这类异常一般是文章中使用了now( ), 小括号( )属于特殊字符,在编译文章时将now( )当函数处理了,结果找不到函数,就报错了。
3.解决方案：
	
	将now( )的小括号通过&#40; &#41; 进行转换

ps:

	异常一通过验证 - ok
	其他异常待验证...

参考资料：

1.[异常处理](https://blog.csdn.net/chwshuang/article/details/52350559)

2.[Markdown语法 - 特殊字符](https://blog.csdn.net/chwshuang/article/details/52350551)

3.[Markdown语法](https://blog.csdn.net/chwshuang/article/details/52350551)