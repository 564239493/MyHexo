---
title: hexo发布带图片博客
date: 2017-08-23 20:28:54
tags: [技术,hexo]
---
### 前言
*** 
参考资料：http://www.jianshu.com/p/cf0628478a4e
### 正文
*** 
1.修改hexo博客项目根目录_config.yml配置文件post_asset_folder项为true。

2.hexo new "hexo发布带图片博客"

3.在source/_post文件夹里面就会出现一个“hexo发布带图片博客.md”的文件和一个“hexo发布带图片博客”的文件夹。

4.引用图片：
{% asset_img 引用图片代码.png 由于hexo会把代码解析，所以此处用图片展示 %} 

### 实例
*** 
> CSDN肯定看不见，来这吧，https://564239493.github.io/

{% asset_img Archer主题.png Archer主题页面 %}