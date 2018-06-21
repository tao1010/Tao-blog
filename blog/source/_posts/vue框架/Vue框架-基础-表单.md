---
title: Vue框架-基础-表单
toc: true
date: 2018-06-21 09:11:33
tags: vue
categories: Web

---

一、基础用法<br>
使用 v-model 指令在表单 &lt;input&gt; 及 &lt;textarea&gt; 元素上创建双向数据绑定；

<!-- more -->
		
	它会更具控件类型自动选取正确的方法来更新元素；
	v-model负责监听用户的输入事件以更新数据，并对一些极端场景进行一些特殊处理；
tips:

	v-model 会忽略所有表单元素的value、checked、selected特性的初始值；
	v-model总是将Vue实例的数据作为数据来源，应该通过JavaScript在组件的 data 选项中声明初始值；
	需要使用输入法的语言，v-model不会在输入法组合文字过程中得到更新，如需处理此问题，使用 input 事件；
	v-model绑定的值通过是静态字符串,对于复选框也可以是布尔值；
1.文本

	<body>
		<div id="example1">
	        <input v-model='message' placeholder="编辑">
	        <p>Message is: {{message}}</p>
	    </div>
	    
	    <script>
	    	var vm = new Vue({
	            el: "#example1",
	            data:{
	                message: '测试一下'
	            }
	        });
	    </script>
	</body>

2.多行文本
	
	<body>
		<div id="example2">
	        <span>Multiline message is:</span>
	        <p style="white-space: pre-line;">{{ message }}</p>
	        <br>
	        <textarea v-model='message' placeholder="add multiple lines"></textarea>
	    </div>
	    <script>
	    	var vm2 = new Vue({
	            el: "#example2",
	            data: {
	                message: ''
	            }
	        });
	    </script>
	</body>
文本区域插值(&lt;textarea&gt;&lt;/textarea&gt;)不会生效,请用v-model代替<br>
3.复选框

	<body>
		<div id="example3">
	        <p>单个复选框，绑定到布尔值:</p>
	        <input type="checkbox" id="checkbox" v-model='checked'>
	        <label for="checkbox">{{ checked}}</label>
	        
	        <p>多个复选框，绑定到同一个数组:</p>
	        <input type="checkbox" id="Jack"  value="Jack" v-model="checkedNames">
	        <label for="Jack">Jack</label>
	        <input type="checkbox" id="John" value="John" v-model="checkedNames">
	        <label for="John">John</label>
	        <input type="checkbox" id="Mike" value="Mike" v-model="checkedNames">
	        <label for="Mike">Mike</label>
	        <br>
	        <span>Checked names: {{ checkedNames }}</span>
	    </div>
	    <script>
	    	var vm3 = new Vue({
	            el: "#example3",
	            data: {
	                checked: false,
	                checkedNames: []
	            }
	        });
	    </script>
	</body>
	
4.单选框

	<body>
		<div id="example4">
	        <p>单选框</p>
	        <input type="radio" id="one" value="One" v-model='picked'>
	        <label for="one">One</label>
	        <input type="radio" id="two" value="Two" v-model='picked'>
	        <label for="two">Two</label>
	        <br>
	        <span>Picked:{{ picked }}</span>
	    </div>
	    <script>
	    	 var vm4 = new Vue({
	            el: "#example4",
	            data: {
	                picked: ''
	            }
	        });
	    </script>
	</body>

5.选择框

	<body>
		<div id="example5">
	        <p>单选</p>
	        <select v-model="selected">
	            <option value="" disabled>请选择</option>
	            <option >A</option>
	            <option >B</option>
	            <option >C</option>
	        </select>
	        <span>Selected: {{selected}}</span>
	        <br>
	        <p>多选时：绑定到一个数组</p>
	        <select v-model="selectedArray" multiple style="width: 80px;">
	           <option value="A">A</option> 
	           <option value="B">B</option>
	           <option value="C">C</option>
	        </select>
	        <br>
	        <span>Selected: {{selectedArray}}</span>
	        <br>
	        <p>用 v-for 渲染的动态选项</p>
	        <select v-model="selectedDefalut">
	            <option v-for="option in options" v-bind:value="option.value">
	                {{ option.text}}
	            </option>
	        </select>
	        <span>Selected: {{selectedDefalut}}</span>
	    </div>
	    <script>
	    	var vm5 = new Vue({
	            el: "#example5",
	            data: {
	                selected: '',
	                selectedArray: [],
	                selectedDefalut: 'A',
	                options: [
	                    {text: 'One', value: "A"},
	                    {text: 'Two', value: 'B'},
	                    {text: 'Three', value: 'C'}
	                ]
	
	            }
	        });
	    </script>
	</body>
tips：
	
	单选时：
		v-model表达式的初始值未能匹配任何选项，<select>元素将被渲染为“未选中”状态，推荐：提供一个值为空的禁用选项

二、值绑定<br>
使用v-bind实现把值绑定到Vue实例的一个动态属性上，并且这个值可以不是字符串。<br>
1.复选框

	<div id="example6">
        <input type="checkbox" v-model="toggle" true-value="yes" false-value="no">
        <label for="toggle">{{toggle}}</label>
    </div>
    
    var vm6 = new Vue({
        el: "#example6",
        data: {
            toggle: 'yes'
        }
    });
2.单选按钮

	 <div id="example6">
        <input type="radio" v-model="pick" v-bind:value="a">
        <label for="pick">{{pick}}</label>
    </div>
    var vm6 = new Vue({
        el: "#example6",
        data: {
            a: "hello",
            pick: ''
        }
    });
3.选择框的选项
	
	  <div id="example6">
        <select v-model="selected">
            <!-- 内联对象字面量 -->
            <option v-bind:value="{number: 123}">123</option>
        </select>
    </div>
    
     var vm6 = new Vue({
        el: "#example6",
        data: {
            selected: ''
        }
    });
三、修饰符<br>
.lazy

	在默认情况下，v-model 在每次 input 事件触发后将输入框的值与数据进行同步 (除了上述输入法组合文字时)。
	也可以添加 lazy 修饰符，从而转变为使用 change 事件进行同步        
.number

	想自动将用户的输入值转为数值类型，可以给 v-model 添加 number 修饰符;
.trim

	要自动过滤用户输入的首尾空白字符，可以给 v-model 添加 trim 修饰符;
eg:

	<body>
		<div id="example7">
	        <!-- 在“change”时而非“input”时更新 -->
	        <input v-model.lazy="msg" placeholder="使用 change 事件进行同步">
	        <br>
	        <input v-model.number="msg" placeholder="输入值转为数值类型">
	        <br>
	        <input v-model.trim="msg" placeholder="自动过滤收尾空白字符">
	    </div>
	    <script>
	    	var vm7 = new Vue({
		        el: "#example7",
		        data: {
		            msg: 'Hello Vue'
		        }
		    });
	    </script>
	</body>
四、组件上使用v-model<br>
[自定义组件](https://cn.vuejs.org/v2/guide/components.html#在组件上使用-v-model)<br>
参考资料:<br>
1.[Vue-表单](https://cn.vuejs.org/v2/guide/forms.html)