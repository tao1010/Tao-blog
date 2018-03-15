---
title: Html基础知识-超链接
date: 2018-03-09 10:02:21
tags: html
---

一、什么是超链接

是互联网的一个功能，使互联网成为互联的网络。超链接使我们能够将我们的文档链接到任何其他文档（或其他资源），也可以链接到文档的指定部分，我们可以在一个简单的网址上提供应用程序（与必须先安装的本地应用程序或其他东西相比）。几乎任何网络内容都可以转换为链接，点击（或激活）超链接将使网络浏览器转到另一个网址（URL）。

tips:
	
	URL可以指向HTML文件、文本文件、图像、文本文档、视频和音频文件以及可以在网络上保存的任何其他内容。 
	如果浏览器不知道如何显示或处理文件，它会询问您是否要打开文件（需要选择合适的本地应用来打开或处理文件）或下载文件（以后处理它）。

二、链接的解析

通过将文本（或其他内容，见[块级链接](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Introduction_to_HTML/Creating_hyperlinks#块级链接))转换为&lt;a&gt;元素内的链接来创建基本链接， 给它一个[href](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/a#attr-href)属性（也称为目标），它将包含您希望链接指向的网址。

1.文本链接：

``` html
<p>I'm creating a link to 
<a href="https://www.mozilla.org/en-US/">the Mozilla homepage</a>.
</p>
```
使用title属性添加支持信息

``` html
<p>I'm creating a link to 
<a href="https://www.mozilla.org/en-US/" title="The best place">the Mozilla homepage</a>.
</p>
```

	包含关于链接的补充有用信息;
	当链接悬停在其上时，标题将作为工具提示出现;
	
tips:

	连接的标题仅当鼠标悬停在其上时才会显示，这意味着使用键盘来导航网页的人很难获取到标题信息。如果标题信息对于页面非常重要，你应该使用所有用户能都方便获取的方式来呈现，例如放在常规文本中。

2.块级链接：

可以将一些内容转换为链接，甚至是 块级元素。

eg1：如果你想要将一个图像转换为链接，你只需把图像放到&lt;a&gt;&lt;/a&gt;标签中间.

``` html
 <a href="https://baidu.com" title="这是百度网站">
            <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1520485528900&di=f29017b61829b0780d22a298835b046a&imgtype=0&src=http%3A%2F%2Fwww.lgstatic.com%2Fthumbnail_160x160%2Fi%2Fimage%2FM00%2F00%2FDE%2FCgqKkVZYK2-AHWKrAAJZ0aVbFEA607.png" alt="网络图片">
            </a>
```

eg2:多个段落的内容为链接，你只需把该段内容放到&lt;a&gt;&lt;/a&gt;标签中间.
 
 ``` html
 <a href="https://baidu.com" title="这是百度网站">
	    <p>It was a dark night. Somewhere, the ...</p>
	    
	    <h2>Chapter 2: The eternal silence</h2>
	    
	    <p>Our protagonist out of the shadowy figure ...</p>
 </a>
 ```
 
三、统一资源定位器(URL)与路径(path)快速入门

1.统一资源定位器（Uniform Resource Locator,简写URL）是一个定义了在网络上的位置的一个文本字符串。URL使用路径查找文件。路径指定文件系统中您感兴趣的文件所在的位置。

eg:目录结构

![目录结构](directory-structure.png)

	此目录结构的根目录称为creation-hyperlinks。当在网站上工作时， 你会有一个包含整个网站的目录。
	
	在根目录，我们有一个index.html和一个contacts.html文件。在真实的网站上，index.html 将会成为我们的主页或登录页面。

	根目录还有两个目录—— pdfs 和projects，它们分别包含一个 PDF (project-brief.pdf) 文件和一个index.html 文件。请注意你可以有两个index.html文件，前提是他们在不同的目录下，许多网站就是如此。第二个index.html或许是项目相关信息的主登录界面。
	
		指向相同目录：
			如果index.html（顶层的index.html）想要包含一个超链接指向contacts.html，你只需要指定想要链接的文件名，因为它与当前文件是在同一个目录的. 所以你应该使用的URL是contacts.html:
			<p>Want to contact a specific staff member?Find details on our <a href="contacts.html">contacts page</a>.</p>
			
		指向子目录：
			如果你想要包含一个超链接到index.html （顶层index.html)）指向 projects/index.html，您需要进入项目目录，然后指明要链接到的文件。 通过指定目录的名称，然后是正斜杠，然后是文件的名称。因此您要使用的URL是projects / index.html：
			<p>Visit my <a href="projects/index.html">project homepage</a>.</p>
			
		指向上级目录：
			如果你想在projects/index.html中包含一个指向pdfs/project-brief.pdf的超链接，你必须返回上级目录，然后再回到pdf目录。“返回上一个目录级”使用两个英文点号表示 — .. — 所以你应该使用的URL是 ../pdfs/project-brief.pdf：
			<p>A link to my <a href="../pdfs/project-brief.pdf">project brief</a>.</p>

tips：

	如果需要的话，你可以将这些功能的多个例子和复杂的url结合起来。例如：../../../complex/path/to/my/file.html.

2.文档片段

超链接可以链接到html文档的特定部分（被称为文档片段），而不仅仅是文件的顶部。步骤：
	
	1.首先分配一个id属性的元素到链接。通常链接到一个特定的标题是有意义的，所以这看起来像下面的内容：

``` html
<h2 id="Mailing_address">Mailing address</h2>
```

	2.然后链接到那个特定的id，您可以在URL的结尾包含它，前面是一个井号（#），例如：
	
``` html
<p>Want to write us a letter? Use our <a href="contacts.html#Mailing_address">mailing address</a>.</p>
```

可以用它自己的文档片段参考链接到同一份文件的另一部分:(跳转到指定位置)

``` html
<p>The <a href="#Mailing_address">company mailing address</a> can be found at the bottom of this page.</p>
```
	
3.绝对链接和相对链接

绝对URL：

	指向由其在Web上的绝对位置定义的位置，包括 协议 and 域名.
	总是指向相同的位置，不管它在哪里使用；
	
eg：
	
	如果index.html 页面上传到projects这一个目录 。project位于web服务站点的根目录, web站点的域名为http://www.example.com, 这个页面可以通过http://www.example.com/projects/index.html访问 ( 或者仅仅通过http://www.example.com/projects/来访问, 因为大多数web服务通过访问index.html这样的页面来加载，如果没有特定的URL的话)

相对URL：

	指向与您链接的文件相关的位置，更像我们在前面一节中所看到的位置。
	eg :
		如果我们想从示例文件链接http://www.example.com/projects/index.html转到相同目录下的一个PDF文件, URL就是文件名URL — 例如 project-brief.pdf —没有其他的信息要求. 如果PDF文件能够在projects的子目录pdfs中访问到, 相对路径就是pdfs/project-brief.pdf (对应的绝对URL就是 http://www.example.com/projects/pdfs/project-brief.pdf.)
	
	一个相对URL将指向不同的位置，这取决于它所在的文件所在的位置。
	eg:
		如果我们把index.html 文件 从 projects 目录移动出来并进入Web站点的根目录（最高级别，而不是任何目录中），  pdfs/project-brief.pdf 的相对URL将会指向http://www.example.com/pdfs/project-brief.pdf, 而不是http://www.example.com/projects/pdfs/project-brief.pdf.
	

4.用清晰的链接措辞

	使用屏幕阅读器的用户喜欢从页面上的一个链接跳到另一个链接，并且脱离上下文来阅读链接。
	搜索引擎使用链接文本为索引目标文件所以，在链接文本中包含关键词是一个很好的主意，以有效地描述与之相关的信息。
	读者往往会浏览页面而不是阅读每一个字，他们的眼睛会被页面的特征所吸引，比如链接。他们会找到描述性的链接。

tips：

	不要重复URL作为链接文本的一部分 — URL看起来很丑，当屏幕朗读器一个字母一个字母的读出来的时候听起来就更丑了。
	不要在链接文本中说“link”或“links to”——它只是噪音。屏幕阅读器告诉人们有一个链接。可视化用户也会知道有一个链接，因为链接通常是用不同的颜色设计的，并且存在下划线（这个惯例一般不应该被打破，因为用户习惯了它。）
	保持你的链接标签尽可能短-长链接尤其惹恼屏幕阅读器用户，他们必须听到整件事读出来。

5.尽可能使用相对链接

当链接到同一网站的其他位置时，你应该使用相关链接（当链接到另一个网站时，你需要使用绝对链接）；
	
	首先，检查代码要容易得多——相对URL通常比绝对URL短得多，这使得阅读代码更容易。
	其次，在可能的情况下使用相对URL更有效。当使用绝对URL时，浏览器首先通过查询域名（使用“DNS”）}查找服务器的真实位置，然后再转到该服务器并查找所请求的文件。另一方面，相对URL，浏览器只在同一服务器上查找被请求的文件。因此，如果你使用相对URL做的绝对URL，你就不断地让你的浏览器做额外的工作，这意味着它的效率会降低。
	
6.链接到非html资源-留下清晰的指示

当链接到一个需要下载的资源（如PDF或Word文档）或流媒体（如视频或音频）或有另一个潜在的意想不到的效果（打开一个弹出窗口，或加载Flash电影），你应该添加明确的措辞，以减少任何混乱。

	如果你是在低带宽连接，点击一个链接，然后就开始下载大文件。
	如果你没有安装Flash播放器，点击一个链接，然后突然被带到一个需要Flash的页面。

7.下载链接时使用下载属性

当您链接到要下载的资源而不是在浏览器中打开时，您可以使用下载属性来提供一个默认的保存文件名。下面是一个下载链接到Firefox 39 Windows版本的示例：

```html
<a href="https://download.mozilla.org/?product=firefox-39.0-SSL&os=win&lang=en-US"
   download="firefox-39-installer.exe">
  Download Firefox 39 for Windows
</a>
```

四、电子邮件链接

当点击一个链接或按钮时，打开一个新的电子邮件发送信息而不是连接到一个资源或页面，这种情况是可能做到的。这样做是使用&lt;a&gt;元素和mailto：URL的方案。
其最基本和最常用的使用形式为一个mailto:link （链接），链接简单说明收件人的电子邮件地址。

``` html
<a href="mailto:nowhere@mozilla.org">Send email to nowhere</a>
```

	邮件地址甚至是可选的。如果你忘记了（也就是说，你的href仅仅只是简单的"mailto:"），一个新的发送电子邮件的窗口也会被用户的邮件客户端打开，只是没有收件人的地址信息，这通常在“分享”链接是很有用的，用户可以发送给他们选择的地址邮件.

1.具体细节

任何标准的邮件头字段可以被添加到邮件的URL你提供。 其中最常用的是主题(subject)、抄送(cc)和主体(body) (这不是一个真正的头字段，但允许您为新邮件指定一个短内容消息)。 每个字段及其值被指定为查询项。

``` html
<a href="mailto:nowhere@mozilla.org?cc=name2@rapidtables.com&bcc=name3@rapidtables.com&amp;subject=The%20subject%20of%20the%20email &amp;body=The%20body%20of%20the%20email">
  Send mail with cc, bcc, subject and body
</a>

<a href="mailto:dengtao_dev@163.com?cc=1083683360@qq.com&subject=this is mail&body=hellowrld">email</a>
```

tips:

	每个字段的值必须是URL编码的。 也就是说，不能有非打印字符（不可见字符比如制表符、换行符、分页符）和空格 percent-escaped. 同时注意使用问号（?）来分隔主URL与参数值，以及使用&符来分隔mailto:中的各个参数。 这是标准的URL查询标记方法。

	mailto:

	mailto:nowhere@mozilla.org

	mailto:nowhere@mozilla.org,nobody@mozilla.org

	mailto:nowhere@mozilla.org?cc=nobody@mozilla.org

	mailto:nowhere@mozilla.org?cc=nobody@mozilla.org&subject=This%20is%20the%20subject

参考资料：

1.[超链接](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Introduction_to_HTML/Creating_hyperlinks)

2.[Percent-encoding](http://en.wikipedia.org/wiki/Percent-encoding)

3.[The GET method](https://developer.mozilla.org/en-US/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data#The_GET_method)