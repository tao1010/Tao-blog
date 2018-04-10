---
title: CSS-调试
date: 2018-03-18 08:59:57
tags: CSS
categories: Web

---

一、CSS和调试

1.和HTML一样，CSS是宽容的,在CSS情况下，如果一个声明是无效的(语法错误或者浏览器不支持该特性),浏览器完全忽略它，然后转向它找到的下一个;

如果选择器是无效的，那么它就不会选择任何东西，而整个规则也不会再做任何事情，浏览器只会继续执行下一个规则。


2.检查DOM和CSS

所有的web浏览器都提供了帮助您检查和理解web页面的开发工具：DOM检查器和CSS编辑器。

DOM检查器

![DOM](DOM.png)

CSS编辑器

![CSS](CSS.png)

Tips:每个属性旁边还有一个复选框，可以选择checked/unchecked来启用/禁用您的CSS属性。这对于找出可能导致错误的原因也非常有用。


3.CSS验证

我们已经看到了[W3C HTML验证器](https://validator.w3.org/)，但是它们也有一个[CSS验证器](http://jigsaw.w3.org/css-validator/)。它的工作方式是相同的，允许您在[特定的URL](http://jigsaw.w3.org/css-validator/#validate_by_uri)上验证CSS，通过[上传本地文件](http://jigsaw.w3.org/css-validator/#validate_by_upload)，或者[直接使用CSS输入](http://jigsaw.w3.org/css-validator/#validate_by_input)。

[CSS Lint](http://csslint.net/)是一个类似的工具，用于验证CSS和发现错误，它也可以提供一些有用的提示并给出有趣的警告。


参考资料：

1.[CSS调试](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Introduction_to_CSS/Debugging_CSS)