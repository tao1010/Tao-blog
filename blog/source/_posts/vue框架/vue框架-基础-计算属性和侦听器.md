---
title: vue框架-基础-计算属性和侦听器
date: 2018-06-15 14:42:49
tags: vue
categories: Web
---

	对于任何复杂逻辑，都应当使用计算属性;
1.基础例子

<!--more-->

```html
<body>
    <div id="example">
        <p>Original message: "{{ message}}"</p>
        <p>Computed reversed message: "{{ reversedMessage }}"</p>
    </div>

    <!-- JavaScript 代码需要放在尾部（指定的HTML元素之后） -->
    <script>
        var vm = new Vue({
            el: '#example',
            data: {
                message: 'Hello'
            },
            computed: {
                 // 计算属性的 getter
                reversedMessage: function(){
                    // `this` 指向 vm 实例
                    return this.message.split('').reverse().join('')
                }
            }

        })
    </script>
</body>
```
	
	解析:
		声明了一个计算属性 reversedMessage。提供的函数将用作属性 vm.reversedMessage 的 getter 函数;
		打开浏览器的控制台，自行修改例子中的 vm。vm.reversedMessage 的值始终取决于 vm.message 的值;
	
2.计算属性缓存 VS 方法
	
```html
<script>
        var vm = new Vue({
            el: '#example',
            data: {
                message: 'Hello'
            },
            computed: {
                 // 计算属性的 getter
                reversedMessage: function(){
                    // `this` 指向 vm 实例
                    return this.message.split('').reverse().join('')
                }
            },
            methods: {
                //方法
                reversedMessage: function(){
                    return this.message.split('').reverse().join('')
                }
            }
        })
    </script>
```

	解析：
		计算属性和方法得到的结果完全相同;
		计算属性是基于它们的依赖进行缓存的,只有在它的相关依赖发生改变时才会重新求值,意味着：
			只要message没有发生改变，多次访问reversedMessage 计算属性会立即返回之前的计算结果，不必在此执行函数;
		每当触发重新渲染时,调用的方法总会再次执行函数;
		
使用场景：
	
	缓存场景:
		当一个性能开销较大的计算属性A，需要遍历一个巨大的数组并做大量计算.如果有其他的计算属性依赖于A,若无缓存，将多次执行A的getter。
	
	若不希望有缓存，请用方法替代。	
3.计算属性 VS 侦听属性<br>
	
	<div id="demo">{{ fullName }}</div>
侦听属性:

	var vm = new Vue({
	  el: '#demo',
	  data: {
	    firstName: 'Foo',
	    lastName: 'Bar',
	    fullName: 'Foo Bar'
	  },
	  watch: {
	    firstName: function (val) {
	      this.fullName = val + ' ' + this.lastName
	    },
	    lastName: function (val) {
	      this.fullName = this.firstName + ' ' + val
	    }
	  }
	})

计算属性:
	
	var vm = new Vue({
	  el: '#demo',
	  data: {
	    firstName: 'Foo',
	    lastName: 'Bar'
	  },
	  computed: {
	    fullName: function () {
	      return this.firstName + ' ' + this.lastName
	    }
	  }
	})
4.计算属性的setter
	
	计算属性默认只有getter,但在需要时可提供setter
	eg：
		HTML
		<div id="demo">{{ fullName}}</div>
		
		JS
		<script>
	        var vm = new Vue({
	            el: '#demo',
	            data: {
	                firstName: 'Foo',
	                lastName: 'Bar',
	            },
	            computed: {
	                fullName: {
	                    get: function(){
	                        return this.firstName + ' ' + this.lastName
	                    },
	                    set: function(){
	                        var names = newValue.split(' ')
	                        this.firstName = names[0]
	                        this.lastName = names[names.length - 1]
	                    }
	                }
	            }
	        })
	    </script>
		
5.侦听器
	
	虽然计算属性在大多数情况下更合适，但有时也需要一个自定义的侦听器。这就是为什么 Vue 通过 watch 选项提供了一个更通用的方法，来响应数据的变化。当需要在数据变化时执行异步或开销较大的操作时，这个方式是最有用的
	
参考资料:<br>
1.[Vue.js中文官网](https://cn.vuejs.org)<br>