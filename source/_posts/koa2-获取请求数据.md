---
title: koa2-获取请求数据
date: 2018-04-12 20:25:35
tags: [koa2]
---

# 零、对象

**<font color=#A52A2A size=4 >ctx.request**</font>是context经过封装的请求对象，<font color=#A52A2A size=4 >**ctx.req**</font>是context提供的node.js原生HTTP请求对象。

<br/>

同理<font color=#A52A2A size=4 >**ctx.response**</font>是context经过封装的响应对象，<font color=#A52A2A size=4 >**ctx.res**</font>是context提供的node.js原生HTTP请求对象。 

<br/>

# 一、GET请求 

在koa中，获取GET请求数据源头是koa中request对象中的query方法或querystring方法。**query返回是格式化好的参数对象，querystring返回的是请求字符串**。

```javascript
const Koa = require('koa')
const app = new Koa()

app.use( async ( ctx ) => {
  let url = ctx.url
  // 从上下文的request对象中获取
  let request = ctx.request
  let req_query = request.query
  let req_querystring = request.querystring
  
	// 从上下文中直接获取
  let ctx_query = ctx.query
  let ctx_querystring = ctx.querystring
  
  ctx.body = {
    url,
    req_query,
    req_querystring,
    ctx_query,
    ctx_querystring
  }
})

app.listen(3000, () => {
  console.log('[demo] request get is starting at port 3000')
})
```

![](koa2-获取请求数据\request-get.png)

<br/>

<!--more--> 

# 二、post请求

**对于POST请求的处理，koa2没有封装获取参数的方法**，需要通过解析上下文context中的原生node.js请求对象req，将POST表单数据解析成**query string**（例如：a=1&b=2&c=3），再将query string 解析成**JSON格式**（例如：{"a":"1", "b":"2", "c":"3"}） 

```javascript
const Koa = require('koa')
const app = new Koa()

app.use( async ( ctx ) => {
	if ( ctx.url === '/' && ctx.method === 'GET' ) {
    // 当GET请求时候返回表单页面
    let html = `
      <h1>koa2 request post demo</h1>
      <form method="POST" action="/">
        <p>userName</p>
        <input name="userName" /><br/>
        <p>nickName</p>
        <input name="nickName" /><br/>
        <p>email</p>
        <input name="email" /><br/>
        <button type="submit">submit</button>
      </form>
    `
    ctx.body = html
    } else if ( ctx.url === '/' && ctx.method === 'POST' ) {
    // 当POST请求的时候，解析POST表单里的数据，并显示出来
    let postData = await parsePostData( ctx )
    ctx.body = postData
  } else {
    // 其他请求显示404
    ctx.body = '<h1>404！！！ o(╯□╰)o</h1>'
  }
})
	
	
// 解析上下文里node原生请求的POST参数
function parsePostData( ctx ) {
  return new Promise((resolve, reject) => {
    try {
      let postdata = "";
      ctx.req.addListener('data', (data) => {
        postdata += data
      })
      
      ctx.req.addListener("end",function(){
        let parseData = parseQueryStr( postdata )
        resolve( parseData )
      })
    } catch ( err ) {
      reject(err)
    }
  })
}
	
	
// 将POST请求参数字符串解析成JSON
function parseQueryStr( queryStr ) {
  let queryData = {}
  let queryStrList = queryStr.split('&')
  console.log( queryStrList )
  for (  let [ index, queryStr ] of queryStrList.entries()  ) {
    let itemList = queryStr.split('=')
    queryData[ itemList[0] ] = decodeURIComponent(itemList[1])
  }
  return queryData
}


app.listen(3000, () => {
  console.log('[demo] request post is starting at port 3000')
})
```

