---
title: hexo+github搭建个人博客
date: 2017-08-18 21:00
tags: [技术,hexo]
---
### 前言
*** 
本来想通过vue.js+nodejs+mongo构建个博客项目，一方面可以实践新技术,另一方面也可以有自己的个人博客，后来因为各种拖延，一直没开始，(lll￢ω￢)，突然看到朋友都在使用git, 也想试试手，刚好又看到大家都用github的page来搭建博客，就来试试，我是采用的hexo+github来实现的，好像还有一个je什么的，截至这边博客前还没有去了解。本文也只是简单写写自己搭建的过程，不是很详细的教学，仅仅是我参照网上资料来实现的，目前，只是简单的能用相关的东西，对于每个技术和知识点都不深入了解，留来后续研究。 
###  搭建
*** 
   1.官网下载最新nodejs安装，新版无需配置环境变量，会帮我们实现，安装后我的node版本：8.3.0 ，npm版本：5.3.0 
   2.（可选）修改npm全局安装路径:
		 npm config set prefix "D:\nodejs\node_global"
		 npm config set cache "D:\nodejs\node_cache"
	3.git下载安装，我是非官网,百度软件中心下载的
	4.npm hexo下载：npm install hexo-cli -g,根据需要决定是否需要添加hexo环境变量（中间有个插叙，我在环境变量path，node的路径后添加了个git的路径，导致node,npm命令失效，将git路径移至node路径前就好了。。。）
	5.hexo init（cd 到创建的blog目录，或者直接此处写blog，则会在当前目录创建blog目录）
	6.npm install
		hexo server（出错）hexo s（此命令本地预览博客，localhost:4000）
		此时若出现如下报错：
		ERROR Local hexo not found in ~/blog
		ERROR Try runing: 'npm install hexo --save'
		则执行命令：
		npm install hexo --save
		若无报错，自行忽略此步骤。

####到此处，最基本的博客项目就好了。接下来就是将博客和github结合：
1.cd到blog文件夹下，打开_config.yml文件
2.打开后往下滑到最后，修改成下边的样子（冒号后记得加空格）：
``` deploy: 
    type: git
    repository: https://github.com/XXX/XXX.github.io.git
    branch: master ```
3.在blog文件夹目录下执行生成静态页面命令：hexo generate        或者：hexo g
4.部署到github命令:hexo deploy            或者：hexo d

hexo deploy命令执行成功后，浏览器中打开网址http://XXX.github.io就能看到了。
到此时，基本框子，就差不多了，

### 后记
*** 
#### 一些过程不在记录
1.github注册，博客仓库的建立
2.hexo主题,发布文章等操作的相关记录

以上过程是在昨天完成的，今天联系了git的相关命令，hexo文章博客的发布等内容，相关博客文章，可以后续补充。（好像我这个版式有点丑啊，第一次markdown编写，(●ˇ∀ˇ●)）

***
>参考内容：[http://www.cnblogs.com/MuYunyun/p/5927491.html#_label1]

