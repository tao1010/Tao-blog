---
title: hexo的常用命令
date: 2018-03-05 14:49:28
tags: hexo
categories: 博客
---

1.建站

<!--more-->


``` bash
hexo init <folder> //新建一个网站
```

``` bash
cd <folder>
```

``` bash
npm install
```

2.新建一篇文章

``` bash
hexo new [layout] <title> //新建一篇文章。如果没有设置 layout 的话，默认使用 _config.yml 中的 default_layout 参数代替。如
果标题包含空格的话，请使用引号括起来。
```
2.1您可以在命令中指定文章的布局（layout），默认为 post，可以通过修改 _config.yml 中的 default_layout 参数来指定默认布局

3.生成静态文件

``` bash
hexo generate //hexo g
```
4.发表草稿

``` bash
hexo publish [layout] <filename>
```
5.部署网站

``` bash
hexo deploy //hexo d //部署之前预先生成静态文件
```

您可执行下列的其中一个命令，让 Hexo 在生成完毕后自动部署网站，两个命令的作用是相同的。

``` bash
hexo deploy --generate // hexo d --g
```
``` bash
hexo generate --deploy // hexo g --d
```

6.监视文件变动

``` bash
hexo watch //hexo w
```

``` bash
hexo generate --watch //Hexo 能够监视文件变动并立即重新生成静态文件，在生成时会比对文件的 SHA1 checksum，只有变动的文件才会写入。
```

7.启动服务器

``` bash
hexo server //hexo s //默认情况下，访问网址为： http://localhost:4000/。
```

8.渲染文件

``` bash
hexo render <file1> [file2] ...
```
[迁移内容](https://hexo.io/zh-cn/docs/migration.html)

9.博客迁移

``` bash
hexo migrate <type> //从其他博客系统 迁移内容。
```

10.清除缓存文件（db.json）和 已生成的静态文件(public)

``` bash
hexo clean <type> //在某些情况（尤其是更换主题后），如果发现您对站点的更改无论如何也不生效，您可能需要运行该命令
```
11.草稿相当于博客私密文章

	命令：
		hexo new draft "new draft"
	会在source/_drafts目录下生成一个new-draft.md文件；这个文件不被显示在页面上，链接也访问不到。也就是说如果你想把某一篇文章移除显示，又不舍得删除，可以把它移动到_drafts目录之中；
	如果你希望强行预览草稿：
		更改配置文件：
			render_drafts: true
		或者，如下方式启动server：
			hexo server --drafts
	把草稿变成文章，或者页面：
	hexo publish [layout] <filename>
	
参考资料：

1.[hexo建站](https://hexo.io/zh-cn/docs/setup.html)	
2.[文章和草稿](https://blog.csdn.net/wizardforcel/article/details/40684575)


