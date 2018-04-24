---
title: HTML-多媒体和嵌入-音频和视频
date: 2018-03-13 09:04:23
tags: html
categories: Web
---

OVPs（在线视频提供商）:YouTube、Dailymotion、Vimeo

在线音频提供商:Soundcloud

一、web中的视频（H5）

1.&lt;video&gt;允许你简单的嵌入一段视频.

``` html
<video src="rabbit320.webm" controls>
  <p>Your browser doesn't support HTML5 video. Here is a <a href="rabbit320.webm">link to the video</a> instead.</p> 
</video>

```

	src:同<img>标签使用方式相同，src属性指向你想要嵌入网页当中的视频资源，他们使用方式完全相同。
	
	controls:用户必须能够控制视频和音频的回放功能。可以使用浏览器提供的控制接口，同时也可以在JavaScript当中使用这些控制接口。(这些媒体应该包括开始、停止、音量调节的功能)
	
&lt;video&gt;标签内的段落 - 后备内容：当浏览器不支持&lt;video&gt;标签的时候，它将会显示出来，它使我们能够对旧的浏览器做一些兼容处理。	
	
2.多格式支持

像 MP3、MP4、WebM这些术语叫做容器格式。他们是用不同的方式来播放音频或者视频的 — 也就是说这些容器是用不同的音频轨道、视频轨道、元数据来呈现媒体文件的。视频和音频都有不同的格式，如下:

	WebM 容器通常包括了 Ogg Vorbis 音频和 VP8/VP9 视频。主要在 FireFox 和 Chrome 当中支持。
	
	MP4 容器通常包括 AAC 以及 MP3 音频和 H.264 视频。主要在 Internet Explorer 和 Safari 当中支持。
	
	老式的 Ogg 容器往往支持 Ogg Vorbis  音频和 Ogg Theora 视频。主要在 Firefox 和 Chrome 当中支持，不过这个容器已经被更强大的 WebM 容器所取代。

音频播放器将会直接播放音频文件,，例如 MP3 和 Ogg 文件。这些不需要容器。（主要用于将音频和视频压缩成可管理的文件（原始的音频和视频文件非常大））

浏览器包含了不同的 Codecs,，如 Vorbis 和 H.264,，它们用来将已压缩的音频和视频转化成二进制数字。正如刚才所说，浏览器并不全支持相同的 codecs，所以你得使用几个不同格式的文件来兼容不同的浏览器。

``` html
<video controls>
    <source src="videoandaudio/rabbit320.mp4" type="video/mp4">
    <source src="videoandaudio/rabbit320.webm" type="video/webm">
    <p>Your browser doesn't support HTML5 video. Here is a <a href="videoandaudio/rabbit320.mp4">link to the video</a> instead.</p>
  </video>

```

	视频应当包括 WebM 和 MP4 两种格式，这两种在目前已经足够支持大多数平台和浏览器。
	每个 <source> 标签页含有一个 type 属性，这个属性是可选的，但是建议你添加上这个属性 — 它包含了视频文件的 MIME types ，同时浏览器也会通过检查这个属性来迅速的跳过那些不支持的格式。如果你没有添加 type 属性，浏览器会尝试加载每一个文件，直到找到一个能正确播放的格式，这样会消耗掉大量的时间和资源。
	
3.&lt;video&gt;其他特性

``` html
<video controls width="400" height="400"
       autoplay loop muted
       poster="poster.png">
  <source src="rabbit320.mp4" type="video/mp4">
  <source src="rabbit320.webm" type="video/webm">
  <p>Your browser doesn't support HTML5 video. Here is a <a href="rabbit320.mp4">link to the video</a> instead.</p>
</video>
```

width和height：
	
	可以用属性控制视频的尺寸，也可以用 CSS 来控制视频尺寸。
	无论使用哪种方式，视频都会保持它原始的长宽比 — 也叫做纵横比。
	如果你设置的尺寸没有保持视频原始长宽比，那么视频边框将会拉伸，而未被视频内容填充的部分，将会显示默认的背景颜色。

autoplay：
	
	自动播放，即使页面其他部分还没加载完全，这个属性会立即播放音视频的内容，建议不添加此属性。
	
loop：

	循环播放音视频文件，不建议使用。
	
muted：
	
	这个属性会导致媒体播放时，默认关闭声音。
	
poster：

	指向一个图像的URL，这个图像会在视频播放前显示。通常用于粗略的预览或者广告。

preload:

	这个属性用来缓冲较大的文件，有三个可选值：
	“none”：不缓冲；
	“auto”：页面加载后缓存媒体文件；
	"metadata":仅缓存文件的元数据
	
	
二、web中的音频

1.&lt;audio&gt;和&lt;video&gt;的使用方式几乎完全相同。细微差别：比如边框等

``` html
<audio controls>
  <source src="viper.mp3" type="audio/mp3">
  <source src="viper.ogg" type="audio/ogg">
  <p>Your browser doesn't support HTML5 audio. Here is a <a href="viper.mp3">link to the audio</a> instead.</p>
</audio>
```

音频播放器所占空间比视频播放器小，没有视觉部分，只需要能控制音频播放的控件：

	不支持width/height属性；
	不支持poster属性

三、显示音轨文本

1.使用场景：听觉障碍者；不能停音频（嘈杂环境、安静环境）；语言不通，需翻译；

2.&lt;track&gt;标签 - HTML5的&lt;video&gt;中的WebVTT格式

WebVTT 是一个格式，用来编写文本文件，这个文本文件包含了众多的字符串，这些字符串会带有一些元数据，它们可以用来描述这个字符串将会在视频中显示的时间，甚至可以用来描述这些字符串的样式以及定位信息。这些字符串叫做 cues ，你可以根据不同的需求来显示不同的样式，最常见的如下：
	
	subtitle:通过添加翻译字幕，来帮助那些听不懂外国语言的人们理解音频当中的内容。
	
	captions:同步翻译对白，或是描述一些有重要信息的声音，来帮助那些不能听音频的人们理解音频中的内容。
	
	timed descriptions:将文字转换为音频，用于服务那些有视觉障碍的人。
	

一个典型的WebVTT文件如下：

``` html
WEBVTT

1
00:00:22.230 --> 00:00:24.606
This is the first subtitle.

2
00:00:30.739 --> 00:00:34.074
This is the second.

  ...

```

3.让WebVTT文件与HTML媒体一起显示的步骤：

	1.以.vtt后缀名保存文件；
	2.用<track>标签链接.vtt文件，<track>标签需放在<audio>或<video>标签当中，同时需要放所有<source>标签之后。
	3.使用 kind 属性来指明是哪一种类型，如 subtitles 、 captions 、 descriptions。然后，使用 srclang 来告诉浏览器你是用什么语言来编写的 subtitles。


``` html
<video controls>
    <source src="example.mp4" type="video/mp4">
    <source src="example.webm" type="video/webm">
    <track kind="subtitles" src="subtitles_en.vtt" srclang="en">
</video>

```

tips:文本轨道会使你的网站更容易被搜索引擎抓取到 （SEO）， 由于搜索引擎的文本抓取能力非常强大，使用文本轨道甚至可以让搜索引擎通过视频的内容直接链接。

	
参考资料：

1.[JavaScript API](https://developer.mozilla.org/en-US/docs/Web/API/HTMLMediaElement)
	
2.[WebVTT](https://developer.mozilla.org/en-US/docs/Web/API/Web_Video_Text_Tracks_Format)
	
3.[kind](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/track#attr-kind)

4.[srclang](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/track#attr-srclang)

5.[Web中的音视频](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Multimedia_and_embedding/Video_and_audio_content)

6.[iframe](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe)

7.[audio](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/audio)

8.[video](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/video)

9.[source](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/source)

10.[track](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/track)
