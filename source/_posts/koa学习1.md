---
title: koa学习1
date: 2017-09-07 22:23
tags: [技术,koa]
---
### 前言
*** 
学习来源：[廖雪峰教程](https://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000/001471087582981d6c0ea265bf241b59a04fa6f61d767f6000)
开发工具：WebStorm

前几天看egg.js，门都入不了，(lll￢ω￢)，放弃，既然egg在koa基础发展而来，那我就看koa，简单记录下练习过程,不包含具体细节。

### 正文
*** 
首先，我们创建一个目录hello_koa并作为工程目录。然后，我们创建app.js，输入以下代码：
```
// 导入koa，和koa 1.x不同，在koa2中，我们导入的是一个class，因此用大写的Koa表示:
const Koa = require('koa');

// 创建一个Koa对象表示web app本身:
const app = new Koa();

// 对于任何请求，app将调用该异步函数处理请求：
app.use(async (ctx, next) => {
    await next();
    ctx.response.type = 'text/html';
    ctx.response.body = '<h1>Hello, koa2!</h1>';
});

// 在端口3000监听:
app.listen(3000);
console.log('app started at port 3000...');
```
因为上一个项目用的express，koa又是基于express发展来的，所以看着这一段代码还是比较好理解的。
对于每一个http请求，koa将调用我们传入的异步函数来处理：
```
async (ctx, next) => {
    await next();
    // 设置response的Content-Type:
    ctx.response.type = 'text/html';
    // 设置response的内容:
    ctx.response.body = '<h1>Hello, koa2!</h1>';
}
```
上面的异步函数中，我们首先用`await next();`处理下一个异步函数，然后，设置response的Content-Type和内容。

### 安装koa
* 可以用npm命令直接安装koa。先打开命令提示符，务必把当前目录切换到hello-koa这个目录，然后执行命令。
* 在hello-koa这个目录下创建一个package.json，这个文件描述了我们的hello-koa工程会用到哪些包。完整的文件内容如下：
```
{
    "name": "hello-koa",
    "version": "1.0.0",
    "description": "Hello Koa 2 example with async",
    "main": "app.js",
    "scripts": {
        "start": "node app.js"
    },
    "keywords": [
        "koa",
        "async"
    ],
    "author": "Michael Liao",
    "license": "Apache-2.0",
    "repository": {
        "type": "git",
        "url": "https://github.com/michaelliao/learn-javascript.git"
    },
    "dependencies": {
        "koa": "2.0.0"
    }
}
```
然后，我们在hello-koa目录下执行npm install就可以把所需包以及依赖包一次性全部装好。
现在，我们的工程结构如下：
```
hello_koa/
|
+- app.js <-- 使用koa的js
|
+- package.json <-- 项目描述文件
|
+- node_modules/ <-- npm安装的所有依赖包
```
### 执行程序
*** 
还可以直接用命令node app.js在命令行启动程序，或者用npm start启动。npm start命令会让npm执行定义在package.json文件中的start对应命令：
```
"scripts": {
    "start": "node app.js"
}
```
### koa middleware
*** 
每收到一个http请求，koa就会调用通过app.use()注册的async函数，并传入ctx和next参数。

我们可以对ctx操作，并设置返回内容。但是为什么要调用await next()？

原因是koa把很多async函数组成一个处理链，每个async函数都可以做一些自己的事情，然后用await next()来调用下一个async函数。我们把每个async函数称为middleware，这些middleware可以组合起来，完成很多有用的功能。

例如，可以用以下3个middleware组成处理链，依次打印日志，记录处理时间，输出HTML：
```
app.use(async (ctx, next) => {
    console.log(`${ctx.request.method} ${ctx.request.url}`); // 打印URL
    await next(); // 调用下一个middleware
});

app.use(async (ctx, next) => {
    const start = new Date().getTime(); // 当前时间
    await next(); // 调用下一个middleware
    const ms = new Date().getTime() - start; // 耗费时间
    console.log(`Time: ${ms}ms`); // 打印耗费时间
});

app.use(async (ctx, next) => {
    await next();
    ctx.response.type = 'text/html';
    ctx.response.body = '<h1>Hello, koa2!</h1>';
});
```
### 处理URL
*** 
此时，随便输入任何URL都会返回相同的网页,我们需要进一步处理：

为了处理URL，我们需要引入koa-router这个middleware，让它负责处理URL映射。
先在package.json中添加依赖项：`"koa-router": "7.0.0"`,然后用npm install安装。

接下来，我们修改app.js，使用koa-router来处理URL：
```
const Koa = require('koa');

// 注意require('koa-router')返回的是函数:
const router = require('koa-router')();

const app = new Koa();

// log request URL:
app.use(async (ctx, next) => {
    console.log(`Process ${ctx.request.method} ${ctx.request.url}...`);
    await next();
});

// add url-route:
router.get('/hello/:name', async (ctx, next) => {
    var name = ctx.params.name;
    ctx.response.body = `<h1>Hello, ${name}!</h1>`;
});

router.get('/', async (ctx, next) => {
    ctx.response.body = '<h1>Index</h1>';
});

// add router middleware:
app.use(router.routes());

app.listen(3000);
console.log('app started at port 3000...');
```
用router.get('/path', async fn)处理的是get请求。如果要处理post请求，可以用router.post('/path', async fn)。

用post请求处理URL时，我们会遇到一个问题：post请求通常会发送一个表单，或者JSON，它作为request的body发送，但无论是Node.js提供的原始request对象，还是koa提供的request对象，都不提供解析request的body的功能！

所以，我们又需要引入另一个middleware来解析原始request请求，然后，把解析后的参数，绑定到ctx.request.body中。

koa-bodyparser就是用来干这个活的。

我们在package.json中添加依赖项：`"koa-bodyparser": "3.2.0"`,然后使用npm install安装

下面，修改app.js，引入koa-bodyparser：
```
const bodyParser = require('koa-bodyparser');
```
```
app.use(bodyParser());
```
> 由于middleware的顺序很重要，这个koa-bodyparser必须在router之前被注册到app对象上。

现在我们就可以处理post请求了。写一个简单的登录表单：
```
router.get('/', async (ctx, next) => {
    ctx.response.body = `<h1>Index</h1>
        <form action="/signin" method="post">
            <p>Name: <input name="name" value="koa"></p>
            <p>Password: <input name="password" type="password"></p>
            <p><input type="submit" value="Submit"></p>
        </form>`;
});

router.post('/signin', async (ctx, next) => {
    var
        name = ctx.request.body.name || '',
        password = ctx.request.body.password || '';
    console.log(`signin with name: ${name}, password: ${password}`);
    if (name === 'koa' && password === '12345') {
        ctx.response.body = `<h1>Welcome, ${name}!</h1>`;
    } else {
        ctx.response.body = `<h1>Login failed!</h1>
        <p><a href="/">Try again</a></p>`;
    }
});
```

此时运行代码，就可以跑起来一个简单的登陆功能，此时状态欠佳，明天再整理重构代码，主要是讲url处理这块提出来优化下。
