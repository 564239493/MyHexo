---
title: hexo访客统计
date: 2017-09-16 23:30
tags: [hexo]
---
### 插件
**** 
访客统计插件: [不蒜子](http://busuanzi.ibruce.info/)
参考：http://ibruce.info/2015/04/04/busuanzi/

### 步骤
*** 
#### 1.引入脚本
```
<script async src="//dn-lbstatics.qbox.me/busuanzi/2.3/busuanzi.pure.mini.js"></script>
```
hexo，打开themes/你的主题/layout/_partial/footer.ejs添加上述脚本即可，当然你也可以添加到 header 中。

#### 2.引入标签
1.显示站点总访问量
要显示站点总访问量，复制以下代码添加到你需要显示的位置。有两种算法可选：

算法a：pv的方式，单个用户连续点击n篇文章，记录n次访问量。
```
<span id="busuanzi_container_site_pv">
    本站总访问量<span id="busuanzi_value_site_pv"></span>次
</span>
```
算法b：uv的方式，单个用户连续点击n篇文章，只记录1次访客数。
```
<span id="busuanzi_container_site_uv">
  本站访客数<span id="busuanzi_value_site_uv"></span>人次
</span>
```
> hexo，打开themes/你的主题/layout/_partial/footer.ejs添加即可。

2.显示单页面访问量
要显示每篇文章的访问量，复制以下代码添加到你需要显示的位置。

算法：pv的方式，单个用户点击1篇文章，本篇文章记录1次阅读量。
```
<span id="busuanzi_container_page_pv">
  本文总阅读量<span id="busuanzi_value_page_pv"></span>次
</span>
```
> 代码中文字是可以修改的，只要保留id正确即可。

### 后记
*** 
文摘，收藏，等标签文章更新
