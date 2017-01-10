---
title: hexo与github page联合建站过程中踩过的坑
date: 2017-01-10 16:15:07
tags: 
	- 技术
	- 建站
---
截止到现在，一直以来的愿望总算达成，虽然距离自己最初的心愿--完全手写一个blog还有很长的路要走。

- **YAML语法**
	
之前没写过配置文件，这是第一次接触YAML，确实简洁强大。

1. key: value; 冒号之后有空格
2. 字符串默认不使用引号。如：title: 欢迎来到我的博客
3. 字符串之中包含空格或特殊字符，需要放在引号之中。如：title: 'hello world'
4. 文章包含多个标签
```
tags:
- one
- two
```
5. #表示注释

具体YAML语法请看[http://www.ruanyifeng.com/blog/2016/07/yaml.html?f=tt](http://www.ruanyifeng.com/blog/2016/07/yaml.html?f=tt)

<!--more-->

-**hexo中相对路径引用的标签插件**

当时有疑问的主要是`{% asset_img slug [title] %}`标签插件,不知道到底应该把图片放在哪里，路径到底是以哪个为根路径。

官方文档中的解释如下
>当你打开文章资源文件夹功能后，你把一个 example.jpg 图片放在了你的资源文件夹中，如果通过使用相对路径的常规 markdown 语法 ![](/example.jpg) ，它将 不会 出现在首页上。（但是它会在文章中按你期待的方式工作）
正确的引用图片方式是使用下列的标签插件而不是 markdown ：
`{% asset_img example.jpg This is an example image %}`
通过这种方式，图片将会同时出现在文章和主页以及归档页中。

现在再看其实很清晰，文档甚至怕使用者看不懂图片的名称都写出来了，然而还是有我这种人不仔细看没看懂。。

资源文件夹指的是当你把资源文件管理功能打开后，即设置
{% codeblock _config.yml %}

post_asset_folder: true

{% endcodeblock %}
每次你通过`hexo new article-title`命令新建文章时，相对应的会生成一个和文章名称相同的文件夹，这个文件夹就是所谓的**资源文件夹**，而根路径也就是这个文件夹。比如
`{% asset_img example.jpg This is an example image %}`

这个`example.jpg`就是直接放在资源文件夹里
{% asset_img example1.jpg This is an example image %}