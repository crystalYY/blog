---
title: hexo与github page联合建站过程中踩过的坑
date: 2017-01-10 16:15:07
tags: 
	- 技术
	- 建站
---
截止到现在，一直以来的愿望总算达成，虽然距离自己最初的心愿--完全手写一个blog还有很长的路要走。

### YAML语法
	
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

### hexo中相对路径引用的标签插件

当时有疑问的主要是 `{ % asset_img slug [title] %}`
标签插件,不知道到底应该把图片放在哪里，路径到底是以哪个为根路径。

官方文档中的解释如下

>当你打开文章资源文件夹功能后，你把一个 example.jpg 
图片放在了你的资源文件夹中，如果通过使用相对路径的常规 markdown 语法 ` ![](/example.jpg) ` ，它将 不会 出现在首页上。（但是它会在文章中按你期待的方式工作）
正确的引用图片方式是使用下列的标签插件而不是 markdown ： `{ % asset_img example.jpg This is an example image %}`
通过这种方式，图片将会同时出现在文章和主页以及归档页中。


现在再看其实很清晰，文档甚至怕使用者看不懂图片的名称都写出来了，然而还是有我这种人不仔细看没看懂。。

资源文件夹指的是当你把资源文件管理功能打开后，即设置
{% codeblock _config.yml %}

post_asset_folder: true

{% endcodeblock %}
每次你通过`hexo new article-title`命令新建文章时，相对应的会生成一个和文章名称相同的文件夹，这个文件夹就是所谓的**资源文件夹**，而根路径也就是这个文件夹。比如 
 `{ % asset_img example.jpg This is an example image %} `
这个`example.jpg`就是直接放在资源文件夹里

{% asset_img example1.jpg This is an example image %}


### github上出现灰色文件

对于这个问题，我找到了原因但依然没有很好地解决办法。

问题产生的原因是由于我作死的直接从原作者的github上pull下来了yilia主题，并且pull的当前文件夹就是hexo的themes文件夹，导致yilia文件夹本身就是一个本身已经初始化并且有远成仓库的本地Git仓库。

用`git remote show origin`来查看远程仓库信息，也是yilia作者的Git远程仓库地址。虽然我后来又在blog文件夹（包含yilia文件夹）init了一个Git本地仓库，并和我github上的远程仓库相连，但我依然得不到yilia文件夹的编辑权，除非到yilia文件夹下，但就算你此时push也是push到yilia作者的仓库里。

查了一些资料，说是yilia这种情况就属于blog的子模块了，子模块是什么鬼，第一次听说。
[子模块资料](https://segmentfault.com/a/1190000003076028)
但我在yilia文件夹里搜索了一下，并没有.gitmodules和 pod-library 文件，所以这应该不是子模块。

那是什么，我又找了一通资料。觉得主要原因是yilia本身也是一个初始化的Git仓库。那取消初始化会不会就正常了，可以执行`git rm rf .git `或直接手动删除.git文件。发现yilia文件确实和blog保持了一致，远程仓库和本地分支都一致，但奈何github上依然是灰色文件不能操作。。

**好吧，最后我无奈的把yilia先复制到桌面上，然后`add commit push`，再把桌面上的yilia恭恭敬敬的又请回了themes文件夹，接着`add commit push`，再去看github，竟然可操作了。。。**

{% asset_img github.png 灰色文件可处理结果 %}

**简单粗暴但有效。**

最后提醒一句：再下载主题的时候别再为了一时方便直接pull主题到博客本身的themes文件夹下了，后续造成的不方便不知道要花费多少时间。当然如果你不打算把hexo的所有配置文件都上传到GitHub上，那倒没太大问题，不过万一哪天你电脑突然挂了呢。。。还得重新再搭建一次，所以还是尽早备份吧
