---
title: CSS-文本样式-网络字体
date: 2018-03-20 09:10:55
tags: CSS
categories: Web
---


一、字体种类回顾

应用到HTML的字体可以使用 font-family属性来控制。这需要一个或多个字体种类名称，浏览器会在列表中搜寻，直到找到它所运行的系统上可用的字体。

	p{
		font-family:Helvetica, "Trebuchets MS", Verdana, sans-serif;
	}
 
[Web-safe字体](https://developer.mozilla.org/en-US/Learn/CSS/Styling_text/Fundamentals#Web_safe_fonts):可以保证所有公共系统中都可以用的字体;

可以使用字体堆栈来指定可选择的字体，后面是Web-safe的替代选项，然后是默认的系统字体，但是为了确保您的设计在每种字体中都可以使用，这增加了测试的开销。

二、Web字体

Web字体是一种CSS特性，允许您指定在访问时随您的网站一起下载的字体文件，这意味着任何支持Web字体的浏览器都可以使用您指定的字体。所需的语法如下所示：

	首先，在CSS的开始处有一个@font-face块，它指定要下载的字体文件：
		@font-face{
			font-family: "myFont";
			src:url("myFont.ttf");
		}
	使用方法：
		html {
		  font-family: "myFont", "Bitstream Vera Serif", serif;
		}
关于网页字体的重要性：
	
	1.浏览器支持不同的字体格式，因此需要多种字体格式以获得良好的跨浏览器支持。
	2.字体一般都不能自由使用。您必须为他们付费，或者遵循其他许可条件，比如在代码中(或者在您的站点上)提供字体创建者。你不应该在没有适当的信用的情况下偷窃字体。

1.查找字体，下载字体

eg:Web字体实例

	HTML:
	
    <h1>Hipster ipsum is the best</h1>

    <p>Tacos actually microdosing, pour-over semiotics banjo chicharrones retro fanny pack portland everyday carry vinyl typewriter. Tacos PBR&amp;B pork belly, everyday carry ennui pickled sriracha normcore hashtag polaroid single-origin coffee cold-pressed. PBR&amp;B tattooed trust fund twee, leggings salvia iPhone photo booth health goth gastropub hammock.</p>

    <p>Cray food truck brunch, XOXO +1 keffiyeh pickled chambray waistcoat ennui. Organic small batch paleo 8-bit. Intelligentsia umami wayfarers pickled, asymmetrical kombucha letterpress kitsch leggings cold-pressed squid chartreuse put a bird on it. Listicle pickled man bun cornhole heirloom art party.</p>

    <h2>It is the quaintest</h2>

    <p>Bespoke green juice aesthetic leggings DIY williamsburg selvage. Bespoke health goth tote bag, fingerstache venmo ennui thundercats butcher trust fund cardigan hella. Wolf vinyl you probably haven't heard of them taxidermy, ugh quinoa neutra meditation asymmetrical mixtape church-key kitsch man bun occupy. Knausgaard butcher raw denim ramps, offal seitan williamsburg venmo gastropub mlkshk cardigan chillwave chartreuse single-origin coffee twee. Ethical asymmetrical banjo typewriter fap. Polaroid waistcoat tousled selfies four dollar toast locavore thundercats. Truffaut post-ironic skateboard trust fund.</p>

    <h2>No, really...</h2>

    <p>Trust fund celiac farm-to-table PBR&amp;B. Brunch art party mumblecore, fingerstache cred echo park literally stumptown humblebrag chambray. Mlkshk vinyl distillery humblebrag crucifix. Mustache craft beer put a bird on it, irony deep v poutine ramps williamsburg heirloom brooklyn.</p>

    <p>Taxidermy tofu YOLO, sustainable etsy flexitarian art party stumptown portland. Ethical williamsburg retro paleo. Put a bird on it leggings yuccie actually, skateboard jean shorts paleo lomo salvia plaid you probably haven't heard of them.</p>
<span>

	html {
	  font-size: 10px;
	  margin: 0;
	  font-family: sans-serif;
	}
	
	body {
	  width: 80%;
	  max-width: 800px;
	  margin: 0 auto;
	}
	
	/* Typography */
	
	h1 {
	  font-size: 3.2rem;
	}
	
	h2 {
	  font-size: 2.4rem;
	}
	
	p {
	  font-size: 1.4rem;
	  line-height: 1.6;
	  word-spacing: 0.6rem;
	}	
<span>上述例子中：

	使用两种web字体，一种用于标题，一种用于正文文本;
	字体是由字体铸造厂创建的，并且存储在不同的文件格式中;	
通常有三种类型的网站可以获取字体：
	
	免费的字体经销商：一个可以下载免费字体的网站;	收费的字体经销商：一个收费则字体可用的网站;
	在线字体服务：一个存储和为你提供字体的网站，它使整个过程更容易;
当您找到每种字体时，按下下载按钮，并将该文件保存在与您先前保存的HTML和CSS文件相同的目录中。

2.生成所需代码

生成所需的代码以及字体样式,步骤如下：

	1）确保您已经满足了任何许可证的要求，如果您打算在一个商业和/或Web项目中使用它。
	2）前往 Font Squirrel(详见参考资料Webfont Generator).
	3）使用上传字体按钮上传你的两个字体文件。
	4）勾选复选框，“是的，我上传的字体符合网络嵌入的合法条件。
	5）点击下载你的套件（kit）。
在生成器完成处理之后，您应该得到一个ZIP文件，将它保存在与HTML和CSS相同的目录中。

3.实现代码

解压上述生成的webfont套件,包含三个有用的条目：
	
	每个字体的对版本：（比如 .ttf, .woff, .woff2…… 随着浏览器支持需求的改变，提供的字体将随着时间的推移而不断更新。） 正如上面提到的，跨浏览器支持需要使用多种字体——这是Fontsquirrel的方法，确保你得到了你需要的一切;
	每个字体的一个演示HTML文件在你的浏览器中加载，看看在不同的使用环境下字体会是什么样子;
	一个stylesheet.css文件,它包含了你需要的生成好的@font-face代码。
实现这些字体，步骤如下：

	1）将解压缩的目录重命名为简易的目录，比如fonts
	2）打开 stylesheet.css 文件，把包含在你的网页中的 @font-face块复制到你的 web-font-start.css 文件—— 你需要把它们放在最上面，在你的CSS之前，因为字体需要导入才能在你的网站上使用。
	3）每个url()函数指向一个我们想要导入到我们的CSS中的字体文件——我们需要确保文件的路径是正确的，因此，在每个路径的开头添加fonts/ （必要时进行调整）。
	4）现在，您可以在字体栈中使用这些字体，就像任何web安全或默认的系统字体一样。
		font-familt: "myFont", serif;
三、使用在线字体服务
	
在线字体服务通常会为你存储和服务字体，这样你就不用担心写@font-face代码了，通常只需要在你的网站上插入一两行代码就可以让一切都运行。例子[Typekit](https://typekit.com/),[Cloud.typography](http://www.typography.com/cloud/welcome/)

步骤：
	
	1）前往 Google Fonts.
	2）使用左边的过滤器来显示你想要选择的字体类型，并选择一些你喜欢的字体。
	3）要选择字体种类，按下按钮旁边的 ⊕ 按钮。
	4）当您选择好字体种类时，按下页面底部的[Number] 种类选择。
	5）在生成的屏幕中，首先需要复制所显示的HTML代码行，并将其粘贴到HTML文件的头部。将其置于现有的<link>元素之上，使得字体是导入的，然后在你的CSS中使用它。
	6）然后，您需要将CSS声明复制到您的CSS中，以便将自定义字体应用到您的HTML。


参考资料：

1.[网络化字体](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/为文本添加样式/Styling_links)

2.[免费字体的网站-Font Squirre](https://www.fontsquirrel.com/)<br>
[Webfont Generator](https://www.fontsquirrel.com/tools/webfont-generator)<br>
[免费字体的网站-Dafont](http://www.dafont.com/)<br>
[免费字体的网站-Everything Fonts](https://everythingfonts.com/)

3.[Google Fonts](https://www.google.com/fonts)