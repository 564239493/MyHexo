---
title: 微信js-sdk本地测试
date: 2017-09-23 23:54
tags: [微信]
---
### 摘要
*** 
因项目中需要使用微信语音相关接口功能，所以需要引入微信的jssdk,但是这个东西的测试不是本地运行那么简单，需要借助微信web开发工具来进行,本文记录了我个人今日完成测试的这个过程，本文的重点不是开发教程，也不是记录wx.jssdk具体使用，而仅是记录，今日的一些流程和遇到的问题。

### 关键点
*** 
1.微信公众号
2.[natapp](https://natapp.cn/) 免费域名 -- 来映射本地前端使用wxjs的服务地址
3.微信公众号  JS接口安全域名设置（这个能把我搞死）
4.后台服务获取wxjs.config所需参数（本人网上下的node demo）
5.前端调用wxjs ( 公司项目 )
6.微信web开发者工具

### 开始
*** 
#### 1. 建立微信公众号
比较简单，自己很轻松就可以实现
#### 2. 前后端代码
因本文重点不是来记录具体代码实现的，所以，此处仅贴出些关键代码。
> 前段时间在学习koa2.0，所以从网上找了微信js签名代码的node demo,将代码拷入我的hello-koa项目，以此来作为后端接口。
```
1.app.js文件
//跨域(新增)
var cors = require('koa-cors');
app.use(cors());

2.router.js文件
var funny = require("./controllers/funnyindex");
//微信认证
router.get('/funny',funny.funny);

3.funnyindex.js
var Jsapi = require("./wechat");
var appid = "你的微信公众号appid";
var appsecret = "你的微信公众号appsecret ";

exports.funny = async (ctx, next) => {
    var callUrl = ctx.request.query.callbackurl;
    const jsapi = new Jsapi(appid, appsecret);
    // //1、获取 access_token, 返回promise对象，resolve回调返回string
    // jsapi.getAccessToken().then(
    //     re => res.end(re)
    // ).catch(err => console.error(err));

    // //2、获取 jsapi_ticket, 返回promise对象，resolve回调返回string
    // jsapi.getJsApiTicket().then(
    //     re => res.end(re)
    // ).catch(err => console.error(err));

    //3、获取 JS-SDK 权限验证的签名, 返回promise对象，resolve回调返回json
    var data = await jsapi.getSignPackage(callUrl).catch(err => console.error(err));
    ctx.body = data;
}

4.wechat.js文件
封装的获取微信签名的各个方法，网上很多，此处不记录了。
```

> 前端调用wx_jssdk的是我的项目，本地服务启动后127.0.0.1：5944/index.html访问，该地址后续会通过natapp映射，作为微信公众号的js接口域名

```
//请求
 console.log("后台获取微信签名");
 $.get("http://127.0.0.1:3009/funny?callbackurl=" + encodeURIComponent(window.location.href),
            {}, setWxConfig);

//设置wx.config（此方法为异步）
function setWxConfig(signPackage) {
    wx.config({
        debug: false,
        appId: signPackage.appId,  //必填,公众号的唯一标识
        timestamp: signPackage.timestamp,             //必填,生成签名的时间戳
        nonceStr: signPackage.nonceStr,             //必填,生成签名的随机串
        signature: signPackage.signature,             //必填,签名,见 http://t.cn/RL24Fgw
        jsApiList: [
            "getNetworkType",
            "getLocation",
            "onMenuShareTimeline",
            "onMenuShareAppMessage",
            'startRecord',//开始录音接口
            'stopRecord',//停止录音接口
            'playVoice',//播放语音接口
            'pauseVoice',//暂停播放接口
            'stopVoice',//停止播放接口
            'uploadVoice',//上传语音接口
            'downloadVoice',//下载语音接口
            'onVoicePlayEnd',
            'translateVoice',//语音识别
        ]
    });
	//以上方法为异步，如果你想在加载的是有调用wxjssdk,就需要用wx.ready，此处不具体说明。
}
```

以上，就算代码类相关的准备处理完毕，开始要处理一些其他的配置

#### 3. JS接口安全域名
>微信公众号 -> 公众号设置 -> 功能设置 -> JS接口安全域名

只有设置了域名,该域名才能调用wxjssdk,那么本地服务，我是通过natapp来映射域名的。通过natapp免费创建个隧道，然后设置ip,port为你的前端服务，就可以，然后下载natapp.exe,根据  [一分钟快速图文教程](https://natapp.cn/article/natapp_newbie)  配置启动该exe就可以.

那么，我原来http://127.0.0.1:5944/index.html的前端就可以通过natapp.exe生成的 http://pgu5g4.natappfree.cc 来访问。将这个域名设为js接口安全域名就可以了。

注意设置安全域名的时候，需要下载MP_verify_gzbQUDassWHFnugA.txt文件放到 http://pgu5g4.natappfree.cc根目录下。

> 微信公众号获取appsecret时还需填写ip白名单,百度自己的ip地址填写就可以

#### 4. 微信web开发者工具
下载该工具，启动前后端服务，在开发者工具页面输入http://pgu5g4.natappfree.cc 就可以了，然后该工具可以看到你的js-sdk请求和权限列表。。

### 呼。。。
*** 
弄了一天，前面一直毫无头绪，各种尝试，一度要放弃，那时候，就是卡在页面一直提示 
>config:invalid url donmain

各种百度，都说时js接口安全域名不对，我也是搞不懂，一直在那里写的后端的域名，醉的一塌糊涂，出去吃了个饭，突然想到这块，发现不对，应该是写前端域名莫。。。

就这样，写累了，88.
