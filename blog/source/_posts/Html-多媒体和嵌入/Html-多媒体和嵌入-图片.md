---
title: Html-多媒体和嵌入-图片
date: 2018-03-12 16:15:31
tags: html
---

1.推荐相对路径加载图片；
	
	绝对路径会让浏览器做更多的工作，充新寻找DNS再寻找IP地址。
通常我们都会把图片和HTML放在同一个服务器上。

搜索引擎也能读取图片的文件名并把它们记入SEO。因此给图片取个描述行的名文件名很重要；

2.像&lt;img&gt;和&lt;video&gt;这样的元素有时被称之为替换元素，因为这样的元素的内容和尺寸由外部资源（像是一个图片或视频文件）所定义，而不是元素自身。


3.alt属性:它的值应该对图片的文字描述，用于在图片无法显示或不能被看到的情况。

	• 用户有视力障碍，通过屏幕阅读器来浏览网页 。事实上，给图片一个备选的描述文本对大多数用户都是很有用的。
	• 就像上面所说的，你也许会把图片的路径或文件名拼错。浏览器不支持该图片类型。某些用户仍在使用纯文本的浏览器，例如 Lynx，这些浏览器会把图片替换为描述文本。
	• 你会想提供一些文字描述来给搜索引擎使用，例如搜索引擎可能会将图片的文字描述和查询条件进行匹配。
	• 用户关闭的图片显示以减少数据的传输，这在手机上是十分普遍的，并且在一些国家带宽是有限且昂贵。


alt里面写什么：
	
	装饰：如果图片只是用于装饰，而不是内容的一部分，可以写一个空的alt="" 。
	内容：如果你的图片提供了重要的信息，就要在alt文本中简要的提供相同的信息，甚至更近一步，把这些信息写在主要的文本内容里，这样所有人都能看见。
	链接：如果你把图片嵌套在<a>标签里，来把图片变成链接，那你还必须提供无障碍的链接文本。在这种情况下，你可以写在同一个<a>元素里，或者写在图片的alt属性里，随你喜欢。
	文本：你不应该将文本放到图像里。例如，如果你的主标题需要有阴影，你可以用CSS来达到这个目的，而不是把文本放到图片里。如果真的必须这么做，那就把文本也放到alt里。


4.宽度和高度属性

``` html
<img src="images/dinosaur.jpg"
     alt="it has a large head with long sharp teeth"
     width="400"
     height="341">

```

图片未加载完成时，浏览器在为图片留下一定的空间。

ps：

	如果你需要改变图片的尺寸，你应该使用CSS而不是HTML；
	使用HTML属性来改变图片的大小，若太大，图片模糊；若太小，下载远远大于你需要的图片时浪费带宽；长宽比例异常，图片显示扭曲；

5.图片标题

图片标题并不必须要包含有意义的信息，通常来说，将这样的支持信息放到主要文本中而不是附着于图片会更好。

``` html
<img src="images/dinosaur.jpg"
     alt="it has a large head with long sharp teeth"
     width="400"
     height="341"
     title="A T-ester University Museum">
     
```
6.解说图片 

&lt;figure&gt; 和&lt;figcaption&gt;为图片提供一个容器，在标题和图片之间建立清晰的关联。

``` html
<figure>
          <img src="https://raw.githubusercontent.com/mdn/learning-area/master/html/multimedia-and-embedding/images-in-html/dinosaur_small.jpg"
               alt="it has a long sharp teeth"
               width="400"
               height="341"
          >
          <figcaption>text mA T-R.</figcaption>
        </figure>

```

ps:

	从无障碍的角度来说，说明文字和 alt 文本扮演着不同的角色。看得见图片的人们同样可以受益于说明文字，而 alt 文字只有在图片无法显示时才这样。 所以，说明文字和 alt  的内容不应该一样，因为当图片无法显示时，它们会同时出现。


&lt;figure&gt;里面不一定是一张图片，只要是一个独立内容单元：

	• 用紧凑、易于掌握的方式表达你的意图。
	• 可以放在页面线性流的中几个地方（Could go in several places in the page's linear flow）
	• 为主要内容提供重要的补充说明。
	
&lt;figure&gt; 可以是几张图片、一段代码、音视频、方程、表格或别的。

7.CSS背景图片

也可以使用 CSS 把图片嵌入网站中（JavaScript也行，不过那是另外一个故事了），这个 CSS 属性 background-image 和另其他 background-* 属性是用来放置背景图片的。

eg：页面中的所有段落设置一个背景图片

``` html
p{
	background-image:url("https://raw.githubusercontent.com/mdn/learning-area/master/html/multimedia-and-embedding/images-in-html/dinosaur_small.jpg")
}

```

总而言之，如果图像对您的内容里有意义，则应使用HTML图像。 如果图像纯粹是装饰，则应使用CSS背景图片。


参考资料：

1.[图片](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Multimedia_and_embedding/Images_in_HTML)