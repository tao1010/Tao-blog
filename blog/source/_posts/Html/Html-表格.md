---
title: Html-表格
date: 2018-03-14 11:24:45
tags:	html
categories: Web
---

一、表格概念
	
	表格是由行和列组成的结构化数据集(表格数据)，它能够使你简捷迅速地查找某个表示不同类型数据之间的某种关系的值。

![表格](numbers-table.png)

1.表格如何工作

	表格的一个特点就是严格. 通过在行和列的标题之间进行视觉关联的方法，就可以让信息能够很简单地被解读出来。

2.表格风格
	
	为了能够让表格在网页上有效, 你需要提供一些 CSS 的样式信息，以及尽可能好的 HTML 固定结构.

3.HTML表格的应用

	HTML 表格 应该用于表格数据;

4.使用表格布局而不是用CSS布局的理由：

	表格布局减少了视觉受损的用户的可访问性: 屏幕阅读器, 被盲人所使用, 解析存在于 HTML 页面上的标签，然后为用户读出其中的内容。因为对于布局来说，表格不是一个正确的工具， 使用的标记比使用 CSS 布局技术更复杂, 所以屏幕阅读器的输出会让他们的用户感到困惑。
	表格会产生很多标签: 正如刚才提到的, 表格布局通常会比正确的布局技术涉及更复杂的标签结构，这会导致代码变得更难于编写、维护、调试.
	表格不能自动响应: 当你使用正确的布局容器 (比如 <header>, <section>, <article>, 或是 <div>), 它们的默认宽度是父元素的 100%. 而表格的的默认大小是根据其内容而定的。因此，需要采取额外的措施来获取表格布局样式，以便有效地在各种设备上工作。

二、创建表格步骤

1.定义table样式

方式一：CSS	
	
	html {
	    font-family: sans-serif;
	}
	table {
	    border-collapse: collapse;
	    border: 2px solid rgb(200,200,200);
	    letter-spacing: 1px;
	    font-size: 0.8rem;
	}
	td, th {
	    border: 1px solid rgb(190,190,190);
	    padding: 10px 20px;
	}
	th {
		background-color: rgb(235,235,235);
	}
	td {
		text-align: center;
	}
	tr:nth-child(even) td {
		background-color: rgb(250,250,250);
	}
	tr:nth-child(odd) td {
		background-color: rgb(245,245,245);
	}
	caption {
		padding: 10px;
	}
方式二：在html的&lt;head&gt;&lt;/head&gt;中加入代码：

	<style>
	    html {
	        font-family: sans-serif;
	    }
	    table {
	        border-collapse: collapse;
	        border: 2px solid rgb(200,200,200);
	        letter-spacing: 1px;
	        font-size: 0.8rem;
	    }
	    td, th {
	        border: 1px solid rgb(190,190,190);
	        padding: 10px 20px;
	    }
	    th {
	    	background-color: rgb(235,235,235);
	    }
	    td {
	    	text-align: center;
	    }
	    tr:nth-child(even) td {
	    	background-color: rgb(250,250,250);
	    }
	    tr:nth-child(odd) td {
	    	background-color: rgb(245,245,245);
	    }
	    caption {
	    	padding: 10px;
	    }
	</style>
2.在html的&lt;body&gt;&lt;/body&gt;中加入标签代码：

	<body>
		<table>
			...
		</table>
	</body>	
3.在表格中，最小的内容容器是单元格, 是通过 <td> 元素创建的 ('td' 代表 'table data'). 把下面的内容添加到你的表格标签中:

	<td>Hi, I'm your first cell.</td>
4.如果我们想要一行四个单元格，我们需要把这组标签拷贝三次，更新你表中的内容，让它看起来是这样的:

	<td>Hi, I'm your first cell.</td>
	<td>I'm your second cell.</td>
	<td>I'm your third cell.</td>
	<td>I'm your fourth cell.</td>

单元格不会放置在彼此的下方, 而是自动与同一行上的其他单元格对齐. 每个 <td> 元素 创建一个单独单元格，它们共同组成了第一行。我们添加的每个单元格都使行的长度变长。

5.新增一行：

如果想让这一行停止增加，并让单元格从第二行开始，我们需要使用 <tr> 元素 ('tr' 代表 'table row'). 

	<tr>
  		<td>Hi, I'm your first cell.</td>
  		<td>I'm your second cell.</td>
  		<td>I'm your third cell.</td>
 		<td>I'm your fourth cell.</td>
	</tr>
可以继续增加至两行、三行。每一行都需要一个额外的&lt;tr&gt;元素来包装，每个单元格的内容都应该写在&lt;td&gt;中。

三、给table添加各种样式结构

1.添加标题:&lt;caption&gt;Dogs table&lt;/caption&gt;

![numbers-table-header](numbers-table-header.png)

	<table>
		  <caption>Dogs table</caption>
		  ...
	</table>
	
2.表格中的标题是特殊的单元格，通常在行或列的开始处，定义行或列包含的数据类型;

为了将表格的标题在视觉上和语义上都能被识别为标题，你可以使用&lt;th&gt;元素 ('th' 代表 'table header'). 用法和&lt;td&gt;是一样的，除了它表示为标题，不是普通的单元格以外。

	 <table>
        <caption>Dogs table</caption>
        <tr>
            <td>&nbsp;</td>
            <th>Knocky</th>
            <th>Flor</th>
            <th>Ella</th>
            <th>Juan</th>
         <tr>
            <th>Breed</th>
            <td>Jack Russell</td>
            <td>Poodle</td>
            <td>Streetdog</td>
            <td>Cocker Spaniel</td>
         </tr>
         <tr>
            <th>Age</th>
            <td>16</td>
            <td>9</td>
            <td>10</td>
            <td>5</td>
         </tr>
         <tr>
            <th>Owner</th>
            <td>Mother-in-law</td>
            <td>Me</td>
            <td>Me</td>
            <td>Sister-in-law</td>
         </tr>
         <tr>
            <th>Eating Habits</th>
            <td>Eats everyone's leftovers</td>
            <td>Nibbles at food</td>
            <td>Hearty eater</td>
            <td>Will eat till he explodes</td>
         </tr>
    </table>

作用：

	当标题明显突出的时候，你可以更加简单地找到你想找的数据，设计上也会看起来更好；
	即使你不给表格添加你自己的样式，表格标题也会带有一些默认样式：加粗和居中，让标题可以突出显示；
	
3.添加&lt;thead&gt;、&lt;tfoot&gt;、&lt;tbody&gt;

	 <thead> 需要嵌套在 table 元素中，放置在头部的位置，因为它通常代表第一行，第一行中往往都是每列的标题，但是不是每种情况都是这样的。如果你使用了 <col>/<colgroup> 元素，那么 <thead>元素就需要放在它们的下面。

	 <tfoot> 需要嵌套在 table 元素中，放置在底部 (页脚)的位置，一般是最后一行，往往是对前面所有行的总结，比如，你可以按照预想的方式将<tfoot>放在表格的底部，或者就放在 <thead> 的下面。(浏览器仍将它呈现在表格的底部)
	
	 <tbody> 需要嵌套在 table 元素中，放置在 <thead>的下面或者是 <tfoot> 的下面，这取决于你如何设计你的结构。(<tfoot>放在<thead>下面也可以生效.)
	
![table结构](table-structure.png)	
	
	 <style>
        tbody {
          font-size: 90%;
          font-style: italic;
        }
        tfoot {
          font-weight: bold;
        }
    </style>
    
    <table>
        <caption>How I chose to spend my money</caption>
        <thead>
            <tr>
                <th>Purchase</th>
                <th>Location</th>
                <th>Date</th>
                <th>Evaluation</th>
                <th>Cost (€)</th>
            </tr>
        </thead>
        <tfoot>
            <tr>
                <th colspan="4">SUM</th>
                <th>118</th>
            </tr>
        </tfoot>
        <tbody>
            <tr>
                <td>Haircut</td>
                <td>Hairdresser</td>
                <td>12/09</td>
                <td>Great idea</td>
                <td>30</td>
            </tr>
            <tr>
                <td>Lasagna</td>
                <td>Restaurant</td>
                <td>12/09</td>
                <td>Regrets</td>
                <td>18</td>
            </tr>
            <tr>
                <td>Shoes</td>
                <td>Shoeshop</td>
                <td>13/09</td>
                <td>Big regrets</td>
                <td>65</td>
            </tr>
            <tr>
                <td>Toothpaste</td>
                <td>Supermarket</td>
                <td>13/09</td>
                <td>Good</td>
                <td>5</td>
            </tr>
        </tbody>
    </table>
四、单元格跨越多行和多列

1.表格中的标题和单元格有 colspan 和 rowspan 属性；这两个属性接受一个没有单位的数字值，数字决定了它们的宽度或高度是几个单元格。

比如:
	
	colspan="2" 该单元格占2个单元格的宽度；//<th colspan="2">Animals</th>
	rowspan="2" 该单元格占2个单元格的高度；//<th rowspan="2">Chicken</th>

![more-row-more-col](more-row-more-col.png)

	<table>
        <caption>多行多列demo</caption>
            <tr>
              <th colspan="2">Animals</th>
            </tr>
            <tr>
              <th colspan="2">Hippopotamus</th>
            </tr>
            <tr>
              <th rowspan="2">Horse</th>
              <td>Mare</td>
            </tr>
            <tr>
              <td>Stallion</td>
            </tr>
            <tr>
              <th colspan="2">Crocodile</th>
            </tr>
            <tr>
              <th rowspan="2">Chicken</th>
              <td>Cock</td>
            </tr>
            <tr>
              <td>Rooster</td>
            </tr>
    </table>
2.为表格中的列提供共同的样式

HTML有一种方法可以定义整列数据的样式信息：就是&lt;col&gt;和&lt;colgroup&gt;元素。 它们存在是因为如果你想让一列中的每个数据的样式都一样，那么你就要为每个数据都添加一个样式，这样的做法是令人厌烦和低效的。你通常需要在列中的每个&lt;td&gt;或&lt;th&gt;上定义样式，或者使用一个复杂的选择器，比如 :nth-child()。

![共同样式](common-style.png)

方法一：不可取

	<table>
		  <tr>
		    	<th>Data 1</th>
		    	<th style="background-color: yellow">Data 2</th>
		  </tr>
		  <tr>
		   	 	<td>Calcutta</td>
		    	<td style="background-color: yellow">Orange</td>
		  </tr>
		  <tr>
		    	<td>Robots</td>
		    	<td style="background-color: yellow">Jazz</td>
		  </tr>
	</table>
方法二：推荐

	<table>
		  <colgroup>
		    	<col>//第一列不处理
		    	<col style="background-color: yellow">//第二列 背景颜色为黄色
		  </colgroup>
		  <tr>
		    	<th>Data 1</th>
		    	<th>Data 2</th>
		  </tr>
		  <tr>
		    	<td>Calcutta</td>
		    	<td>Orange</td>
		  </tr>
		  <tr>
		    	<td>Robots</td>
		    	<td>Jazz</td>
		  </tr>
	</table>

	使用了两个 <col>来定义“列的样式”，每一个<col>都会制定每列的样式，对于第一列，没有采取任何样式，但是仍然需要添加一个空的 <col> 元素，如果不这样做，那么得到的样式就会应用到第一列上，这和预想的不一样。
	表示从此行开始的"2"行一致：<col span="2">

如果你想把这种样式信息应用到每一列，我们可以只使用一个&lt;col&gt;元素，不过需要包含 span 属性：

	<colgroup>
		  <col style="background-color: yellow" span="2">
	</colgroup>
	
tips:col中有多个属性样式时，用; 分号分开
	
	<col style=" background-color:#DCC48E; border:4px solid #C1437A">

五、嵌套表格

在一个表格中嵌套另外一个表格是可能的，只要你包含完整的结构，包括&lt;table&gt;元素。这样通常是不建议的，因为这种做法会使标记看上去很难理解，对使用屏幕阅读的用户来说，可访问性也降低了。

![嵌套表格](Nested-tables.png)

	  <table id="table1">
	        <caption>嵌套表格</caption>
	        <tr>
		          <th>title1</th>
		          <th>title2</th>
		          <th>title3</th>
	        </tr>
	        <tr>
		          <td id="nested">
			            <table id="table2">
				              <tr>
					                <td>cell1</td>
					                <td>cell2</td>
					                <td>cell3</td>
				              </tr>
			            </table>
		          </td>
		          <td>cell2</td>
		          <td>cell3</td>
	        </tr>
	        <tr>
		          <td>cell4</td>
		          <td>cell5</td>
		          <td>cell6</td>
	        </tr>
	</table>

六、对于视力受损的用户的表格

1.使用列和行的标题 - &lt;th&gt;（tale header）

2.scope属性：可以添加在&lt;th&gt;元素中，用来帮助屏幕阅读设备更好地理解那些标题单元格，这个标题单元格到底是列标题呢，还是行标题。

	scope 还有两个可选的值 ： colgroup 和 rowgroup。这些用于位于多个列或行的顶部的标题。

将列标题这样定义：
	
	<thead>
		  <tr>
			    <th scope="col">Purchase</th>
			    <th scope="col">Location</th>
			    <th scope="col">Date</th>
			    <th scope="col">Evaluation</th>
			    <th scope="col">Cost (€)</th>
		  </tr>
	</thead>
每一行都可以这样定义一个行标题 (如果我们已经使用了 th 和 td 元素):

	<tr>
		  <th scope="row">Haircut</th>
		  <td>Hairdresser</td>
		  <td>12/09</td>
		  <td>Great idea</td>
		  <td>30</td>
	</tr>	
3.id和标题属性

如果要替代 scope 属性，可以使用 id 和 headers 属性来创造标题与单元格之间的联系。使用方法如下:

	1）为每个<th> 元素添加一个 id (id 不能重复)。
	
	2）为每个 <td> 元素添加一个 headers 属性。每个 headers 属性需要包含 th 元素的 id 。 如果这个单元格是属于一个 th 元素下的，那么就需要包含 th 元素的 id 的值，如果属于多个 th 元素，那么可以用空格分隔这些 id。
	
	
参考资料：

1.[HTML表格基础](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Tables/Basics)

2.[Table表格结构](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Tables/Advanced)