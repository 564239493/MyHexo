---
title: koa学习2
date: 2017-09-08 23:19
tags: [技术,koa]
---
### 前言
*** 
学习来源：[廖雪峰教程](https://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000/001471087582981d6c0ea265bf241b59a04fa6f61d767f6000) ,
开发工具：WebStorm
该文主要记录下我自己URL的处理，参照上海电气node项目的思路来进行。

### 正文
*** 
思路：所有的URL处理函数都放到app.js里显得很乱，而且，每加一个URL，就需要修改app.js。随着URL越来越多，app.js就会越来越长。

如果能把URL处理函数集中到某个js文件，或者某几个js文件中就好了，然后让app.js自动导入所有处理URL的函数。

登陆业务代码：
``` 
//首页
module.exports.index = async (ctx, next) => {
    ctx.response.body = `<h1>Index</h1>
        <form action="/signin" method="post">
            <p>Name: <input name="name" value="koa"></p>
            <p>Password: <input name="password" type="password"></p>
            <p><input type="submit" value="Submit"></p>
        </form>`;
}

//登陆
module.exports.login = async (ctx, next) => {
    var name = ctx.request.body.name || '',
    password = ctx.request.body.password || '';
    console.log(`signin with name: ${name}, password: ${password}`);
    if (name === 'koa' && password === '12345') {
        ctx.response.body = `<h1>Welcome, ${name}!</h1>`;
    } else {
        ctx.response.body = `<h1>Login failed!</h1>
            <p><a href="/">Try again</a></p>`;
    }
}
```

我们在controllers目录下编写router.js
```
//路由
var index = require("./controllers/index");
var hello = require("./controllers/hello");

module.exports = function(router){
    //首页
    router.get('/',index.index);
    //登录
    router.post('/signin',index.login);

    router.get('/hello/:name',hello.hello);

    //此处代码必须放在最后一行，请勿在此代码后添加任何路由
    router.get('*', async (ctx, next) => {
        ctx.response.body = `<h1>404 Page!</h1>`;
    });
}
```

优化app.js
```
//koa
const koa = require('koa');
//参数解析
const bodyParser = require('koa-bodyparser');
// 注意require('koa-router')返回的是函数:
const router = require('koa-router')();

const app = new koa();

// log request URL:
app.use(async (ctx, next) => {
    console.log(`Process ${ctx.request.method} ${ctx.request.url}...`);
    await next();
});

//参数解析器
app.use(bodyParser());

//url路由
require("./router")(router);

// add router middleware:
app.use(router.routes());

app.listen(3000);
console.log('app started at port 3000...');
```

这样，我们以后只需要再controllers里编写具体的业务代码，再router.js里注册路由URL,感觉这样比较简练清晰。

### 后记
*** 
下一节，练习模板引擎Nunjucks结合koa的使用，应该还是简单的登陆页面，然后成功失败页面。