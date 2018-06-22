---
title: Vue框架-组件-基础
toc: true
date: 2018-06-22 09:11:46
tags: vue
categories: Web

---

一、组件基础<br>	
1.基本示例

	组件是可复用的Vue实例，与new Vue接收相同的选项(data、computed、watch、methods、生命周期钩子等)，仅有像el这样根实例特有的选项。
	
<!-- more -->

	<body>
	    <div id="app">
	        <p>自定义组件</p>
	        <button-counter></button-counter>
	    </div>
	    <!-- JavaScript 代码需要放在尾部（指定的HTML元素之后） -->
	    <script>
	        
	       // 定义一个名为button-counter 的新组件
	       Vue.component('button-counter', {
	           data: function(){
	               return {
	                   count: 0
	               }
	           },
	           template: "<button v-on:click='count++'> You clicked me {{ count }} times.</button>"
	       })
	       var vm = new Vue({
	           el: '#app'
	       });
	    </script>
	</body>

	tips：
		在<script></script>中，创建自定义组件在前，使用在后：
			1.Vue.component('xxx', {...});
			2.var vm = new Vue({...})
	
2.组件复用<br>
一个组件的 data 选项 必须是一个函数

	组件复用时，每个组件都会各自独立维护它的count，因为每用一次组件，就会有一个新的实例被创建
	每个实例维护一份被返回对象的独立的拷贝
3.组件的组织

	通常一个应用会以一颗嵌套的组件树的形式来组织；
	组件必须先注册以便Vue能够识别：
		全局注册和局部注册
		全局注册的组件可以用在其被注册之后的任何(通过new Vue)新创建的Vue根实例，包括组件树中的所有子组件的模版中。
4.通过Prop向子组件传递数据
	
	一个组件默认可以拥有任意数量的prop，任何值都可以传递给任何prop
	可以使用 v-bind 来动态传递prop
	eg:
	<div id="app2">
        <blog-post title="My journey with Vue"></blog-post>
        <blog-post title="Blogging with Vue"></blog-post>
        <blog-post title="Why Vue is so fun"></blog-post>
        <br>
        <blog-post v-for='post in posts' v-bind:key='post.id' v-bind:title='post.title'></blog-post>
    </div>
    <!-- JavaScript 代码需要放在尾部（指定的HTML元素之后） -->
    <script>
        
       // 定义一个名为button-counter 的新组件
       Vue.component('blog-post',{
           props:['title'],
           template: '<h3>{{title}}</h3>'
       });
       var vm1 = new Vue({
           el: "#app2",
           data: {
               posts: [
                   {id: 1,title: 'My journey with Vue'},
                   {id: 2,title: 'Blogging with Vue'},
                   {id: 3,title: 'Why Vue is so fun'}
               ]
           }
       });
    </script>
5.单个根元素

	每个组件必须只有一个根元素；
	不论何时为 post 对象添加一个新的属性，它都会自动地在 <blog-post> 内可用；
	需要在不 (经过 Babel 或 TypeScript 之类的工具) 编译的情况下支持 IE，请使用折行转义字符取而代之；
	
	eg:修改自定义组件 template
	Vue.component('blog-post', {
		  props: ['post'],
		  template: `
		    <div class="blog-post">
		      <h3>{{ post.title }}</h3>
		      <div v-html="post.content"></div>
		    </div>
		  `
	})
6.通过事件向父级组件发送消息<br>
eg:增大博文的字号	
	
	自定义组件：
	Vue.component('blog-post',{
       props:['post'],
       template: `
        <div class="blog-post">
            <h3>{{ post.title }}</h3>
            <button v-on:click="$emit('enlarge-text')">
                Enlarge text
            </button>
            <div v-html="post.content"></div>
        </div>
        `
    });
	在其父组件中，通过添加一个 postFontSize 数据属性来支持这个功能；
	var vm1 = new Vue({
       el: "#app2",
       data: {
           posts: [
               {id: 1,title: 'My journey with Vue'},
               {id: 2,title: 'Blogging with Vue'},
               {id: 3,title: 'Why Vue is so fun'}
           ],
           postFontSize: 1
       }
	});
	在模版中控制所有博文的字号：
	<div :style = "{fontSize: postFontSize + 'em'}">
        <blog-post v-for='post in posts' 
                   v-bind:key='post.id' 
                   v-bind:post='post'
                   v-on:enlarge-text="postFontSize +=0.1">
        </blog-post>
    </div>
	调用内建的 $emit 方法并传入事件的名字，来向父级组件触发一个事件;
	自定义组件中:
		<button v-on:click="$emit('enlarge-text')">
            Enlarge text
        </button>
	
	用 v-on 在组件上监听这个事件，就像监听一个原生 DOM 事件一样;
	使用组件时：
		 v-on:enlarge-text="postFontSize +=0.1"
使用事件抛出一个值
	
	组件决定文本要放大多少，使用 $emit 的第二个参数提供
	
	在父级组件监听这个事件，通过 $even 访问被抛出的值
	

在组件上使用v-model

	自定义事件也可以用于创建支持 v-model 的自定义输入组件；
7.通过插槽分发内容<br>
[插槽](https://cn.vuejs.org/v2/guide/components-slots.html)<br>
8.动态组件<br>
[动态和异步组件](https://cn.vuejs.org/v2/guide/components-dynamic-async.html)

9.解析DOM模板时的注意事项
	
	有些 HTML 元素，诸如 <ul>、<ol>、<table> 和 <select>，对于哪些元素可以出现在其内部是有严格限制的;
	有些元素，诸如 <li>、<tr> 和 <option>，只能出现在其它某些特定的元素内部;
	
	如果我们从以下来源使用模板的话，这条限制是不存在的：
		字符串 (例如：template: '...')
		单文件组件 (.vue)
		<script  type="text/x-template">
	
参考资料:<br>
1.[Vue - 组件](https://cn.vuejs.org/v2/guide/components.html)<br>
2.[单文件组件](https://cn.vuejs.org/v2/guide/single-file-components.html)<br>