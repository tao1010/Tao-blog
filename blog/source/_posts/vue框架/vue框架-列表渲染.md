---
title: vue框架-列表渲染
date: 2018-06-15 14:47:39
tags: vue
categories: Web
---

1.用v-for把一个数组对应为一组元素

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

5.对象更新检测

6.显示过滤/排序结果

7.一段取值范围的v-for

8.v-for on a &lt;template&gt;

9.v-for 和 v-if

10.一个组件的v-for



七、事件处理<br>
八、表单输入绑定<br>
参考资料:<br>
1.[Vue.js中文官网](https://cn.vuejs.org)<br>