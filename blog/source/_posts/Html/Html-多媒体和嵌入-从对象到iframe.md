---
title: HTML-多媒体和嵌入-从对象到iframe
date: 2018-03-13 11:40:06
tags: html
categories: Web
---

一、嵌入的简介

&lt;iframe&gt;: 用于嵌入其他网页；

&lt;embed&gt;和&lt;object&gt;允许嵌入PDF、SVG、Flash

发展：框架集、小模块-->嵌入Flash等（&lt;embed&gt;和&lt;object&gt;） -->&lt;iframe&gt;嵌入（&lt;canvas&gt;和&lt;video&gt;）

二、嵌入类型的使用

``` html
<iframe width="420" height="315" src="https://www.baidu.com" frameborder="0" allowfullscreen>
</iframe>
```

1.&lt;iframe&gt;基本要素：

	1.allowfullscreen:如果设置，则可以使用全屏API放置在全屏模式

	2.frameborder:如果设置为1，则会告诉浏览器在此框架和其他框架之间绘制边框，这是默认行为。0删除边框。不推荐这样设置，因为在CSS中可以更好地实现相同的效果。border: none;
	
	3.src：该属性与<video>/<img>一样包含指向要嵌入的文档的URL的路径。
	
	4.width和height：这些属性指定您想要的iframe的宽度和高度。


	5.备选内容：与<video>等其他类似元素相同，您可以在打开和关闭<iframe></iframe>标签之间包含备选内容，如果浏览器不支持，将会显示<iframe>。在这种情况下，我们已经添加了一个链接到页面。您几乎不可能遇到任何不支持<iframe>的浏览器。
	
	6.sandbox：该属性比其他<iframe>功能（例如IE 10及更高版本）稍微更现代的浏览器工作，要求提高安全性设置;
	
	
tips：为了提高速度，在主内容完成加载后，使用JavaScript设置iframe的src属性是个好主意。这使您的页面可以更快地使用，并减少您的官方页面加载时间（重要的SEO指标）。

2.安全隐患

[单击劫持](https://en.wikipedia.org/wiki/Clickjacking)是一种常见的iframe攻击，黑客将隐藏的iframe嵌入到您的文档中（或将您的文档嵌入自己的恶意网站），并使用它来捕获用户的交互。这是误导用户或窃取敏感数据的常见方式。

	a.只有在必要时嵌入
	
	b.使用https：HTTPS减少了远程内容在传输过程中被篡改的机会；HTTPS防止嵌入式内容访问您的父文档中的内容，反之亦然。
	
	c.始终使用sandbox属性：一个代码可以适当使用或用于测试的容器，但不能对其他代码库（意外或恶意）造成任何损害称为沙箱；
	未沙盒化(Unsandboxed)内容可以做得太多（执行JavaScript，提交表单，弹出窗口等）默认情况下，您应该使用没有参数的sandbox属性来强制执行所有可用的限制。
	永远不应该同时添加allow-scripts和allow-same-origin到你的sandbox属性中-在这种情况下，嵌入的内容可以绕过，从执行脚本停止网站同源安全策略，并使用JavaScript来关闭完全沙箱。

	d.配置CSP指令：CSP代表内容安全策略，它提供一组HTTP标头（由web服务器发送时与元数据一起发送的元数据），旨在提高HTML文档的安全性。在<iframe>s安全性方面，您可以将服务器配置为发送适当的X-Frame-Options  标题。
	这样做可以防止其他网站在其网页中嵌入您的内容（这将导致点击和一系列其他攻击），正如我们之前看到的那样，MDN开发人员已经做了这些工作

三、&lt;embed&gt;和&lt;object&gt;元素

这些元素是用来嵌入多种类型的外部内容的通用嵌入工具，其中包括像Java小程序和Flash，PDF（可在浏览器中显示为一个PDF插件）这样的插件技术，甚至像视频，SVG和图像的内容！

tips：插件是一种对浏览器原生无法读取的内容提供访问权限的软件。

它们在一些圈子中仍然受欢迎，我们最好是用链接指向它们，而不是将其嵌入到网页中，以便它们可以在单独的页面上被下载或被阅读。

&lt;canvas&gt;用于JavaScript生成的2D和3D图形；

&lt;svg&gt;用于嵌入矢量图形；



四、网页中添加矢量图形

1.矢量图形概念
	
	位图使用像素网格来定义 — 一个位图文件精确得包含了每个像素的位置和它的色彩信息。流行的位图格式包括 Bitmap (.bmp), PNG (.png), JPEG (.jpg), and GIF (.gif.)
	
	矢量图使用算法来定义 — 一个矢量图文件包含了图形和路径的定义，电脑可以根据这些定义计算出当它们在屏幕上渲染时应该呈现的样子。 SVG 格式可以让我们创造用于 Web 的精彩的矢量图形。

放大网页上的矢量图和位图：位图会出现马赛克的感觉（图形像素化了），矢量图不会变化（算法计算出来的形状）

矢量图形相较于同样的位图，通常拥有更小的体积，因为它们仅需储存少量的算法，而不是逐个储存每个像素的信息。

2.SVG的概念

SVG是用来描述矢量图像的XML语言.用于标记 图形而不是内容。

eg:创建一个圆和一个矩形

```html
<svg version="1.1"
     baseProfile="full"
     width="300" height="200"
     xmlns="http://www.w3.org/2000/svg">
  <rect width="100%" height="100%" fill="black" />
  <circle cx="150" cy="100" r="90" fill="blue" />
</svg>
```

	<circle>和<rect>创建简单的图形
	<feColorMatrix>使用变换矩阵变换颜色
	<animate>矢量图形的动画部分
	<mask> 图像顶部应用蒙版
矢量图像编辑器:[inkscape](https://inkscape.org/en/)或[Illustrator](https://en.wikipedia.org/wiki/Adobe_Illustrator)

3.SVG优缺点：
	
	优：
	矢量图像中的文本仍然可以访问(有利于SEO)；
	SVG可以很好的适应样式/脚本(因为图像的每个组件都是可以通过CSS或通过JavaScript编写的样式的元素);
	
	缺：
	SVG非常容易变得复杂，文件大小会增加，复杂的SVG会在浏览器中占用很长处理时间；
	SVG可能比栅格图像更难创建，具体取决于你尝试创建哪种图像；
	旧版浏览器不支持SVG；（IE9开始支持）

光栅图像更适合照片那样复杂精密的图像。

4.将SVG添加到网页的方法

4.1快捷方式&lt;img&gt;:

```html	
<img 
src="equilateral.svg" 
alt="triangle with all three sides equal"
height="87px"
width="100px" />
```

优点

	快速，熟悉的图像语法与alt属性中提供的内置文本等效。
	可以通过在<a>元素嵌套<img>，使图像轻松地成为超链接。
缺点

	无法使用JavaScript操作图像。
	如果要使用CSS控制SVG内容，则必须在SVG代码中包含内联CSS样式。 （从SVG文件调用的外部样式表不起作用）
	不能用CSS伪类来重设图像样式（如:focus）。

4.2疑难解答和跨浏览器支持

对于不支持SVG的浏览器(IE8及以下，Android 2.3及以下)，可以从src属性引用PNG或JPG，并使用srcset属性来引用SVG，在此情况下，仅支持浏览器将加载SVG-较旧的浏览器将加载PNG：

``` html
<img src="equilateral.png" alt="triangle with equal sides" srcset="equilateral.svg">
```

还可以使用SVG作为CSS背景图像,旧版浏览器会理解PNG，较新的浏览器将加载SVG：

``` html
background: url("fallback.png") no-repeat center;
background-image: url("image.svg");
background-size: contain;
```
缺：
	
	像上面描述的<img>方法一样，使用 CSS 背景图像插入SVG 意味着它不能被 JavaScript 操作，并且也受到相同的 CSS 限制。
	如果 SVG 根本没有显示，可能是因为你的服务器设置不正确。 如果是这个问题，这篇文章将告诉你正确方向。

4.3在HTML引入SVG代码

在文本编辑器中打开SVG文件，复制SVG代码，粘贴到HTML文档中 - SVG内联或内联SVG

``` html
<svg width="300" height="200">
    <rect width="100%" height="100%" fill="green" />
</svg>
```

``` html
<svg width="100%" height="100%">
    <rect width="100%" height="100%" fill="red" />
    <circle cx="100%" cy="100%" r="150" fill="blue" stroke="black" />
    <polygon points="120,0 240,225 0,225" fill="green"/>
    <text x="50" y="100" font-family="Verdana" font-size="55"
          fill="white" stroke="black" stroke-width="2">
      Hello!
    </text>
  </svg>
```

优点:

	将 SVG 内联减少 HTTP 请求，可以减少加载时间。
	您可以为 SVG 元素分配class和id，并使用 CSS 修改样式，无论是在SVG中，还是 HTML 文档中的 CSS 样式规则。 实际上，您可以使用任何 SVG外观属性 作为CSS属性。
	内联SVG是唯一可以让您在SVG图像上使用CSS交互（如:focus）和CSS动画的方法（即使在常规样式表中）。
	您可以通过将 SVG 标记包在<a>元素中，使其成为超链接。
缺点

	这种方法只适用于在一个地方使用的SVG。多次使用会导致资源密集型维护（resource-intensive maintenance）。
	额外的 SVG 代码会增加HTML文件的大小。
	浏览器不能像缓存普通图片一样缓存内联SVG。
	您可能会在<foreignObject> 元素中包含回退，但支持 SVG 的浏览器仍然会下载任何后备图像。你需要考虑仅仅为支持过时的浏览器，而增加额外开销是否真的值得。

4.4使用&lt;ifrmae&gt;嵌入SVG - 这不是最好的方法

``` html
<iframe src="triangle.svg" width="500" height="500" sandbox>
    <img src="triangle.png" alt="Triangle with three unequal sides" />
</iframe>
```

缺点:

	如你所知， iframe有一个回退机制，如果浏览器不支持iframe，则只会显示回退。
	此外，除非 SVG 和您当前的网页具有相同的 origin，否则你不能在主页面上使用 JavaScript 来操纵 SVG。

五、响应式图片

[CSS是比HTML更好的响应式设计工具](https://cloudfour.com/thinks/responsive-images-101-part-8-css-images/)

0.为什么要用自适应图片

	艺术方向问题
	分辨率切换问题

最近应用的响应式图像技术，通过让浏览器提供多个图像文件来解决上述问题，比如使用相同显示效果的图片但包含多个不同的分辨率（分辨率切换），或者使用不同的图片以适应不同的空间分配（艺术方向）。

创建自适应图片

1.分辨率切换：不同尺寸

标准的<img>元素传统上仅仅让你给浏览器指定唯一的资源文件：
	
	<img src="elva-fairy-800w.jpg" alt="Elva dressed as a fairy">

可以使用两个新的属性——srcset 和 sizes——来提供更多额外的资源图像和提示，帮助浏览器选择正确的一个资源：
	
	<img srcset="elva-fairy-320w.jpg 320w,
             elva-fairy-480w.jpg 480w,
             elva-fairy-800w.jpg 800w"
     sizes="(max-width: 320px) 280px,
            (max-width: 480px) 440px,
            800px"
     src="elva-fairy-800w.jpg" alt="Elva dressed as a fairy">
     
     
 srcset定义了我们允许浏览器选择的图像集，以及每个图像的大小。在每个逗号之前，我们写：

	一个文件名 (elva-fairy-480w.jpg.)
	一个空格
	图像的固有宽度（以像素为单位）（480w）——注意到这里使用w单位，而不是你预计的px。这是图像的真实大小，可以通过检查你电脑上的图片文件找到（例如，在Mac上，你可以在Finder上选择这个图像，然后按 Cmd + I 来显示信息）。
sizes定义了一组媒体条件（例如屏幕宽度）并且指明当某些媒体条件为真时，什么样的图片尺寸是最佳选择—我们在之前已经讨论了一些提示。在这种情况下，在每个逗号之前，我们写：

	一个媒体条件（(max-width:480px)）——你会在 CSS topic中学到更多的。但是现在我们仅仅讨论的是媒体条件描述了屏幕可能处于的状态。在这里，我们说“当视窗的宽度是480像素或更少”。
	一个空格
	当媒体条件为真时，图像将填充的槽的宽度（440px）

	tips：对于槽的宽度，你也许会提供一个固定值 (px, em) 或者是一个相对值（比如百分比）你也许以及注意到最后一个槽的宽度是没有媒体条件的，它是默认的，当没有任何一个媒体条件为真时，它就会生效。 当浏览器成功匹配第一个媒体条件的时候，剩下所有的东西都会被忽略，所以要注意媒体条件的顺序。

有了上述属性，浏览器会：	
	
	查看设备宽度
	检查sizes列表中哪个媒体条件是第一个为真
	查看给予该媒体查询的槽大小
	加载srcset列表中引用的最接近所选的槽大小的图像
	
老旧的浏览器不支持这些特性，会忽略这些特性，并继续正常加载src属性引用的图像文件。

	tips:在HTML文件中的 <head> 标签里，你将会找到这一行代码 <meta name="viewport" content="width=device-width">: 这行代码会强制地让手机浏览器采用它们真实视图的宽度来加载网页（有些手机浏览器会提供不真实的视图宽度, 然后加载比浏览器真实视图的宽度大的宽度的网页，然后再缩小加载的页面，这样的做法对响应式图片或其他设计，没有任何帮助.

2.艺术方向问题

涉及要更改显示的图像以适应不同的图像显示尺寸。&lt;picture&gt;

``` html
<img src="elva-800w.jpg" alt="Chris standing up holding his daughter Elva">

```

``` html
<picture>
	<source media="(max-width: 799px)" srcset="elva-480w-close-portrait.jpg">
	<source media="(min-width: 800px)" srcset="elva-800w.jpg">
	<img src="elva-800w.jpg" alt="Chris standing up holding his daughter Elva">
</picture>

```

	<source>元素包含一个media属性，这个属性包含一个媒体条件（这个条件决定哪张图片会显示--第一个条件返回真，那么就会显示这张图）
	srcset属性包含要显示图片的路径，请注意，正如我们在<img>上面看到的那样，<source>可以使用引用多个图像的srcset属性，还有sizes属性。所以你可以通过一个 <picture>元素提供多个图片，不过也可以给每个图片提供多分辨率的图片。
	在任何情况下，你都必须在 </picture>之前正确提供一个<img>元素以及它的src和alt属性，否则不会有图片显示。当媒体条件都不返回真的时候（你可以在这个例子中删除第二个<source> 元素），它会提供图片；如果浏览器不支持 <picture>元素时，它可以作为后备方案。
	
tips:你应该仅仅当在艺术方向场景下使用media属性；当你使用media时，不要在sizes属性中也提供媒体条件。

3.为什么不用CSS或JavaScript来做自适应图片
	
	当浏览器开始加载一个页面, 它会先下载 (预加载) 任意的图片，这是发生在主解析器开始加载和解析页面的 CSS 和 JavaScript 之前的。
	平均来说，页面加载的时间少了 20%。但是, 这对响应式图片一点帮助都没有, 所以需要实现类似 srcset的方法。
	因为你不能先加载好 <img> 元素后, 再用 JavaScript 检测视图的宽度，如果觉得大小不合适，就动态地加载小的图片替换已经加载好的图片，这样的话, 原始的图像已经被加载了, 然后你也加载了小的图像, 这样的做法对于响应式图像的理念来说，是很糟糕的。

4.Webp和JPEG-2000

``` html
<picture>
  <source type="image/svg+xml" srcset="pyramid.svg">
  <source type="image/webp" srcset="pyramid.webp"> 
  <img src="pyramid.png" alt="regular pyramid built from four equilateral triangles">
</picture>
```

	不要使用media属性，除非你也需要艺术方向。
	在<source> 元素中，你只可以引用在type中声明的文件类型。
	像之前一样，如果必要，你可以在srcset和sizes中使用逗号分割的列表。
5.小结

艺术方向：当你想为不同布局提供不同剪裁的图片——比如在桌面布局上显示完整的、横向图片，而在手机布局上显示一张剪裁过的、突出重点的纵向图片，可以用 <picture> 元素来实现。

分辨率切换：当你想要为窄屏提供更小的图片时，因为小屏幕不需要像桌面端显示那么大的图片；以及你想为高/低分辨率屏幕提供不同分辨率的图片时，都可以通过 vector graphics (SVG images)、 srcset 以及 sizes 属性来实现。

6.一些有用的开发工具

[开发者工具](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_are_browser_developer_tools)

[响应设计视图](https://developer.mozilla.org/en-US/docs/Tools/Responsive_Design_Mode)

[DOM检查](https://developer.mozilla.org/en-US/docs/Tools/Page_Inspector)


参考资料：

1.[从对象到iframe - 其他嵌入技术](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Multimedia_and_embedding/其他嵌入技术)

2.[全屏API](https://developer.mozilla.org/en-US/docs/Web/Apps/Fundamentals/User_notifications/Full_screen_api)

3.[嵌入矢量图形](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Multimedia_and_embedding/Adding_vector_graphics_to_the_Web)

4.[SVG参考元素](https://developer.mozilla.org/zh-CN/docs/Web/SVG/Element)

5.[响应式图片](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Multimedia_and_embedding/Responsive_images)

6.[img](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/img)