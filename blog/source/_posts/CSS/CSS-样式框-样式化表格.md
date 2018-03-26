---
title: CSS-样式框-样式化表格
date: 2018-03-21 14:11:10
tags: CSS

---

举例代码:

``` html
HTML:
<body>
    <table>
        <caption>A summary of the UK's most famous punk bands</caption>
        <thead>
          <tr>
            <th scope="col">Band</th>
            <th scope="col">Year formed</th>
            <th scope="col">No. of Albums</th>
            <th scope="col">Most famous song</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <th scope="row">Buzzcocks</th>
            <td>1976</td>
            <td>9</td>
            <td>Ever fallen in love (with someone you shouldn't've)</td>
          </tr>
          <tr>
            <th scope="row">The Clash</th>
            <td>1976</td>
            <td>6</td>
            <td>London Calling</td>
          </tr>
          <tr>
            <th scope="row">The Damned</th>
            <td>1976</td>
            <td>10</td>
            <td>Smash it up</td>
          </tr>
          <tr>
            <th scope="row">Sex Pistols</th>
            <td>1975</td>
            <td>1</td>
            <td>Anarchy in the UK</td>
          </tr>
          <tr>
            <th scope="row">Sham 69</th>
            <td>1976</td>
            <td>13</td>
            <td>If the kids are united</td>
          </tr>
          <tr>
            <th scope="row">Siouxsie and the Banshees</th>
            <td>1976</td>
            <td>11</td>
            <td>Hong Kong Garden</td>
          </tr>
          <tr>
            <th scope="row">Stiff Little Fingers</th>
            <td>1977</td>
            <td>10</td>
            <td>Suspect Device</td>
          </tr>
          <tr>
            <th scope="row">The Stranglers</th>
            <td>1974</td>
            <td>17</td>
            <td>No More Heroes</td>
          </tr>
        </tbody>
        <tfoot>
          <tr>
            <th scope="row" colspan="2">Total albums</th>
            <td colspan="2">77</td>
          </tr>
        </tfoot>
      </table>
</body>
```

一、间距和布局

```css
CSS1
/* spacing */

table {
  table-layout: fixed;
  width: 100%;
  border-collapse: collapse;
  border: 3px solid purple;
}

thead th:nth-child(1) {
  width: 30%;
}

thead th:nth-child(2) {
  width: 20%;
}

thead th:nth-child(3) {
  width: 15%;
}

thead th:nth-child(4) {
  width: 35%;
}

th, td {
  padding: 20px;
}
```
解析：   
1.table-layout: fixed   
  thead th:nth-child(n)
	
	使表的行为在默认情况下更可预测。通常情况下，表列的大小会根据所包含的内容大小而变化，这会产生一些奇怪的结果。通过 table-layout: fixed，您可以根据标题的宽度来对列进行大小排序，然后根据适当的内容处理它们的内容。
	这就是为什么我们使用了thead th:nth-child(n) 选择了四个不同的标题(:nth-child)选择器（“选择第n个子元素，它是一个<th>元素，在一个<thead>元素中”）并给定了它们的百分比宽度。整个列宽度与标题的宽度是一样的，这是一种很好的方式来对表列进行大小。

2.border-collapse: collapse
	
	默认情况下，当您在表元素上设置边框时，它们之间将会有间隔，如下图所示：
![no-border-collapse](no-border-collapse.png)   
	
	设置上述属性后如下图：
![border-collapse](border-collapse.png)    
3.border: 3px solid purple
	
	在表页眉和页脚后面设置一些边框——当你在表格外面没有一个边界的时候，它看起来很奇怪，而且是不连贯的，最后会有空隙；
4.th,td  padding:20px

	对表格的标题th和单元格td使用间距，使表看起来更清晰
二、一些简单的排版

``` css
CSS2
/* typography */

html {
  font-family: 'helvetica neue', helvetica, arial, sans-serif;
}

thead th, tfoot th {
  font-family: 'Rock Salt', cursive;
}

th {
  letter-spacing: 2px;
}

td {
  letter-spacing: 1px;
}

tbody td {
  text-align: center;
}

tfoot th {
  text-align: right;
}
```
解析:
	
	1.字体调整 - 更易阅读 html字体、thead和tfoot字体
	2.字体间距 标题th和单元格td
	3.<tbody>中的表格单元中对文本进行了居中对齐，使它们与标题对齐。默认情况下，单元格被赋予了一个text-align的left值.
	4.<tfoot>中对标题进行了右对齐
三、图形和颜色

```css
CSS3
/* graphics */
thead, tfoot {
  background: url(leopardskin.jpg);//添加背景图片
  color: white;//设置字体颜色
}

thead th, tfoot th, tfoot td {
  background: linear-gradient(to bottom, rgba(0,0,0,0.1), rgba(0,0,0,0.5));
  border: 3px solid purple;
  text-shadow: 1px 1px 1px black;
}
//斑马条纹图案
//关键字 - 奇数 - odd
//关键字 - 偶数 - even
tbody tr:nth-child(odd) {
  background-color: #ff33cc;
}

tbody tr:nth-child(even) {
  background-color: #e495e4;
}

tbody tr {//为所有的行添加图片
  background-image: url(noise.png);
}

table {
  background-color: #ff33cc;
}
```
四、标题格式化

```css
/* caption */
caption {
  font-family: 'Rock Salt', cursive;
  padding: 20px;
  font-style: italic;
  caption-side: bottom;//
  color: #666;
  text-align: right;
  letter-spacing: 1px;
}
```
解析：

	 caption-side: bottom - 标题被放置在表格的底部

表格样式总结：

	使您的表格标记尽可能简单，并且保持灵活性，例如使用百分比，这样设计就更有响应性。
	使用 table-layout: fixed 创建更可预测的表布局，可以通过在标题width中设置width来轻松设置列的宽度。
	使用 border-collapse: collapse 使表元素边框合并，生成一个更整洁、更易于控制的外观。
	使用<thead>, <tbody>和<tfoot> 将表格分割成逻辑块，并提供额外的应用CSS的地方，因此如果需要的话，可以更容易地将样式层叠在一起。
	使用斑马线来让其他行更容易阅读。
	使用 text-align直线对齐您的<th>和<td>文本，使内容更整洁、更易于跟随。
	
参考资料：   
1.[样式化表格](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Styling_boxes/Styling_tables)