---
title: vue框架-基础-列表渲染
date: 2018-06-15 14:47:39
tags: vue
categories: Web
---

1.用v-for把一个数组对应为一组元素

<!--more-->

	用 v-for 指令根据一组数组的选项列表进行渲染。
	v-for 指令需要使用 item in items 形式的特殊语法，items 是源数据数组并且 item 是数组元素迭代的别名;
	eg1:
 
		HTML:
		 <div>
	        <ul id="app3">
	            <li v-for='item in items'>
	                {{ item.message }}
	            </li>
	        </ul>
	    </div>
	    
	    JS:
	    var vm3 = new Vue({
	        el: '#app3',
	        data: {
	            items: [
	                {message: '牛奶'},
	                {message: '橙汁'},
	                {message: '雪碧'}
	            ]
	        }
	    });
	    控制台:
	    vm3.items.push({message: '可乐'}) 
	在 v-for 块中，拥有对父作用域属性的完全访问权限。v-for 还支持一个可选的第二个参数为当前项的索引：
	eg2：
		HTML:
		<div>
	        <ul id="app4">
	            <li v-for="(item,index) in items">
	                {{ parentMessage }} - {{ index }} - {{ item.message }}
	            </li>
	        </ul>
	    </div>
		JS:
		var vm4 = new Vue({
            el: '#app4',
            data: {
                parentMessage: 'Parent',
                items: [
                    {message: '牛奶'},
                    {message: '橙汁'},
                    {message: '雪碧'}
                ]
            }
        });
    也可以用 of 替代 in 作为分隔符，因为它是最接近 JavaScript 迭代器的语法：
	eg3:
		<div v-for="item of items"></div>

2.一个对象的v-for<br>

<!-- more -->
	可以用 v-for 通过一个对象的属性来迭代:
	eg1:
		HTML:
		<div>
	        <ul id="v-for-object" class="demo">
	            <li v-for="value in object">
	                {{ value }}
	            </li>
	        </ul>
	    </div>
	    
	    JS:
		var vm5 = new Vue({
            el: '#v-for-object',
            data: {
                object: {
                    firstName: 'John',
                    lastName: 'Doe',
                    age: 30
                }
            }
        });
	可以提供第二个参数的键名：
	eg2:
		<div v-for="(value, key) in object">
		  {{ key }}: {{ value }}
		</div>
	
	第三个参数为索引：
	eg3:
		<div v-for="(value, key, index) in object">
		  {{ index }}. {{ key }}: {{ value }}
		</div>
	
在遍历对象时，是按 Object.keys() 的结果遍历，但是不能保证它的结果在不同的 JavaScript 引擎下是一致的.<br>
3.key

	
	
4.数组更新检测<br>
变异方法

	push()
	pop()
	shift()
	unshift()
	splice()
	sort()
	reverse()
	
	在控制台输入上述变异方法：
	eg:example1.items.push({ message: 'Baz' })
替换数组
	
	filter()
	concat()
	slice()
	
	不会改变原始数组，但会返回一个新数组，可以用新数组替换原来的旧数组,Vue 不会 丢弃之前的DOM，重新渲染整个列表，Vue会使用智能的、启发式的方法。
	eg:
		example1.items = example1.items.filter(function (item) {
			  return item.message.match(/Foo/)
		})
tips:
	
	由于JavaScript的限制，Vue不能检测一下变动的数组：
	1.当利用索引直接设置一个项时；
	eg:
		#控制台通过索引直接修改数组中元素的值，浏览器上不响应
		vm.items[0] = '0'
	解决方案:
	#方法一：属性
	vm.items.splice(indexOfItem,1,newvalue)
	eg:vm.items.splice(0,1,'0')
	#方法二：全局方法
	Vue.set(vm.items,indexOfItem,newvalue)
	eg:Vue.set(vm.items,1,'2')
	
	2.当修改数组的长度时；
	解决方法：vm.items.splice(newLength)
	eg:vm.items.splice(4)
	
5.对象更新检测<br>
由于JavaScript的限制，Vue不能检测对象属性的添加或删除：

	解决方法：
	#方法一：全局方法
	Vue.set(object,key,value) 
	eg:#向嵌套对象添加响应式属性
		var vm = Vue({
			data:{
				userInfo:{
					name: 'John'
				}
			}
		})
		#添加age属性
		Vue.set(vm.userInfo,'age',20)
	方法二：实例方法
		vm.$set(vm.userInfo,'age',20)
为已有的对象赋予多个新的属性

	Object.assgin() 或者 _.ectend()
	eg：
	vm.userInfo = Object.assgin({},vm.userInfo,{
		age: 20,
		sex: 'M'
	})
6.显示过滤/排序结果<br>
显示一个数组的过滤或排序副本，不实际改变或重置原始数据:

	<li v-for="n in evenNumbers">{{n}}</li>
	计算属性可用：
	var vm = new Vue({
		data:{
			numbers:[1,2,3,4]
		},
		computed:{
			evenNumbers: function(){
				
				return: this.numbers.filter(function (number){
					return number % 2 ===0
				})
			}
		}
	}) 
	
	计算属性不可以，使用method方法：
	var vm = new Vue({
		data:{
			numbers:[1,2,3,4]
		},
		method:{
			even: function(numbers){
				
				return: numbers.filter(function (number){
					return number % 2 ===0
				})
			}
		}
	})

7.一段取值范围的v-for
	
	v-for 可以取整数，它将重复多次模版
	
8.v-for on a &lt;template&gt;
	
	利用带有v-for的<template>渲染多个元素
	<ul>
	  <template v-for="item in items">
	    <li>{{ item.msg }}</li>
	    <li class="divider" role="presentation"></li>
	  </template>
	</ul>
	
9.v-for 和 v-if

	当v-for和v-if处于同一节点：
		v-for优先级高于v-if，v-if将分别重复运行于每个v-for循环中
	使用场景：想为仅有的一些项 渲染节点
	eg:
		<li v-for="todo in todos" v-if="!todo.isComplete">
			  {{ todo }}
		</li>

	如果想有条件的跳过循环的执行，可以将v-if置于外层元素(或者<template>)上
	eg:
		<ul v-if="todos.length">
		  <li v-for="todo in todos">
		    {{ todo }}
		  </li>
		</ul>
		<p v-else>No todos left!</p>
10.一个组件的v-for

跳过组件...


七、事件处理<br>
八、表单输入绑定<br>
参考资料:<br>
1.[Vue.js中文官网](https://cn.vuejs.org)<br>