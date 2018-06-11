---
title: koa2-中间组件middleware
date: 2018-04-12 19:26:36
tags: [koa2,middleware]
---

# 一、中间组件

Koa2通过**app.use()**把很多<font color=#A52A2A size=4 >**async函数**</font>组成一个<font color=#A52A2A size=4 >**处理链**</font>，每个async函数都可以做一些自己的事情，然后用await next()来调用下一个async函数。**我们把每个async函数称为middleware**。 

```javascript
// 在koa2中，我们导入的是一个class，因此用大写的Koa表示:
const Koa = require('koa');

// 创建一个Koa对象表示web app本身:
const app = new Koa();

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

// 在端口3000监听:
app.listen(3000);
```

![](koa2-中间组件middleware\3663059-03622ea2a9ffce2a.jpg)

<br/>

# 二、数据传递

koa约定了一个**中间件**的存储空间**ctx.state**，通过这个state可以**共享**一些的数据。