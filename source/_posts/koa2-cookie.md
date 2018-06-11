---
title: koa2-cookie
date: 2018-04-12 20:37:25
tags: [koa2,cookie]
---

koa 提供了从上下文直接读取、写入 cookie 的方法 

```javascript
ctx.cookies.get(name, [options])	// 读取上下文请求中的 cookie
ctx.cookies.set(name, value, [options])	 //在上下文中写入 cookie
```

options 的配置如下：

- **signed**：cookie      是否加密（如果加密的话必须用 **app.keys** 指定加密短语）
- **maxAg**e：cookie 有效时长
- **expires**：cookie 何时过期
- **path**：cookie 的路径，默认为 ‘/’
- **domain**：cookie 的域名
- **secure**：cookie 是否只有 https      请求下才发送
- **httpOnly**：是否只有服务器可以去到      cookie，默认为 true

- **overwrite**：是否允许重写

```javascript
const Koa = require('koa');
const app = new Koa();

// 指定加密短语
app.keys = ['secret1', 'secret2'];

app.use(async (ctx) => {
  if(ctx.url === '/index'){
    ctx.cookies.set(
      'cid',
      'hello worls',
      {
        signed:true,
        domain: '127.0.0.1',
        path: '/index',
        maxAge: 10 * 60 * 1000,
        expores: new Date('2017-09-09'),
        httpOnly: false,
        overwrite: false,
        secure: false
      }
    );
    ctx.body = 'cookies is ok';
  } else {
    ctx.body = 'hello world';
  }
});

app.listen(3000, () => {
  console.log('[demo] cookie is starting at port 3000')
})

```

