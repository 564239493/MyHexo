---
title: Hexo主题和博客发布
date: 2017-08-19 22:30
tags: [技术,hexo]
---
### 前言
*** 
本文继上篇Hexo+Github构建个人博客，接着来记录下Hexo博客主题使用和博客文章发布的过程，也顺便练习练习Markdown。
*** 

### Hexo主题应用
***

* 使用Hexo主题：[Archer](https://github.com/fi3ework/hexo-theme-archer)，
* 该主题实例博客展示：http://firework.studio/

个人也是参照该主题教程来做的，一下简单记录下过程。

1.cd到博客项目根目录下：cd D:\nodejs\blog
2.下载主题：
npm install hexo-generator-json-content --save(不知道干嘛的) 
git clone https://github.com/fi3ework/hexo-theme-archer.git themes/archer
3.修改blog目录下的 _config.yml 的 theme 字段为 archer：
` theme: archer `
4.404启用（我没试）
5.sidebar启用（没试）

以上完成皮肤就算有个大概了，接下来进行部分字段修改，就可以正式使用了。
*** 
### Hexo主题配置
*** 
在D:\nodejs\blog\themes\archer路径下一样存在个 _config.yml文件用来修改主题的某些设置。

	# ========== 资料栏 ========== #
	# 头像路径
	avatar:
	# 博主名字，不填写该字段则默认采用Hexo配置文件中的author字段
	author:
	# 博客签名
	signature:
	# 社交账号
	social:
	  email:
	  github:
	  weibo:
	  zhihu:
	  facebook:
	  twitter:
	  instagram:
	  stack-overflow:
	  linkedin:
	  blog:
	# 友链
	friends:
	  friendA:
	  friendB:
	  friendC:
	
	# ========== 站点 ========== #
	# 网站的title，每篇文章后面也会加上此字段利于SEO
	SEO_title:
	# 显示在网站banner上的主标题
	main_title: 
	# 显示在网站banner上的副标题
	subtitle:
	# 主页banner的背景图片
	header_image:
	# 文章页的默认背景图片
	post_header_image:
	# 404页的背景图片
	_404_image:
	
	
	# ========== 评论插件 ========== #
	# 目前只支持一键添加livere评论（因为懒），其他评论插件可自行添加在post.ejs模板中的</main>标签前。
	comment:
	  # 将你的livere的uid填入以下字段
	  livere:
	
	# ========== 统计 ========== #
	# 是否开启不蒜子统计
	busuanzi: false
	# 百度统计(填写siteID)
	baidu_analytics:
	# Google统计(填写siteID)
	google_analytics:
	# CNZZ统计
	CNZZ_analytics:
	
	# ========== 其他 ========== #
	# favicon
	favicon:
	# 首页的文章摘要字数(默认300)
	truncate_length:
	
***
### Hexo 写博客
*** 
> .md == markdown语言编写的文件

在D:\nodejs\blog\source\_posts目录下就是我们博客文章的md文件了，也就是博客原文了。我自己每次新建一篇博客，就是复制一个原来的文档，保留文档的以下内容 ，其他删掉来写正文。

	title: 8.17
	date: 2017-08-17 19:58:54
	tags: 日记

*** 
### 博客主题更新和发布博客
*** 
* 更换主题后需要执行清缓存操作,网页正常情况下可以忽略此条命令：

> hexo clean

* 生成静态网页

>hexo g

* 开启本地预览服务

>hexo s

* 发布线上

>hexo d

此时，打开 [我的github博客] https://564239493.github.io/就可以看见了,O(∩_∩)O
*** 
### 到此结束
*** 
> 1.Markdown编写有待进步
> 2.带图片博客
> 3.Git了解，命令使用
> 惨不忍睹的排版！！！


