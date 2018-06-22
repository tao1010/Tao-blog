---
title: Vue框架-组件-深入了解组件
toc: true
date: 2018-06-22 11:07:53
tags: vue
categories: Web
---

一、组件注册<br>
1.组件名

<!-- more -->

	Vue.componnet('my-componnet-name',{/*...*/})
	第一个参数即为自定义组件名；
	定义组件名：
		1.推荐：使用kebab-case
			字母全部小写，必须包含连字符 ‘-’
			直接在DOM（非字符串的模版）使用只有kebab-case 有效
		2.使用PascalCase(驼峰式命名) 
			首字母大写
2.全局注册


3.局部注册

	全局注册往往是不够理想的：
		当不再使用一个组件时，若为全局注册，仍然会被包含在最终的构建结果中。
	

二、Prop<br>


三、自定义事件<br>


四、插槽<br>


五、动态组件和异步组件<br>


六、处理边界情况<br>

参考资料:<br>
1.[深入了解组件](https://cn.vuejs.org/v2/guide/components-registration.html)<br>