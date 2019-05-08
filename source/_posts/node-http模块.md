---
title: node-http模块
date: 2019-05-08 20:04:54
tags: [nodejs,node,http]
---

#### http模块

nodejs中的http模块中封装了一个HTPP服务器和一个简易的HTTP客户端

http.Server是一个**基于事件**的http服务器

http.request则是一个http客户端工具，用于向http服务器发起请求

<!--more-->

<br/>



#### http.Server

```javascript
var http = require("http");
var server = new http.Server();

// 参数req和res分别是http.IncomingMessage和http.ServerResponse的实例
server.on("request", function(req, res) {
    res.writeHead(200, {
        "content-type": "text/plain"
    });
    res.write("hello nodejs");
    res.end();
});
server.listen(3000);
```

##### 事件

http.Server提供的事件如下：

- request：当客户端请求到来时，该事件被触发，提供两个参数req和res
- connection：当TCP连接建立时，该事件被触发，提供一个参数socket，是net.Socket的实例
- close：当服务器关闭时，触发事件（注意不是在用户断开连接时）

<br/>



##### http.IncomingMessage

http.IncomingMessage是HTTP请求的信息，一般由http.Server的request事件发送，并作为第一个参数传递，http请求一般可以分为两部分：请求头和请求体。其提供了3个事件，如下：

- data：当请求体数据到来时，该事件被触发，该事件提供一个参数chunk，表示接受的数据，如果该事件没有被监听，则请求体会被抛弃，该事件可能会被调用多次
- end：当请求体数据传输完毕时，该事件会被触发，此后不会再有数据
- close：用户当前请求结束时，该事件被触发，不同于end，如果用户强制终止了传输，也是用close

<br/>



##### http.ServerResponse

http.ServerResponse是返回给客户端的信息，一般由http.Server的request事件发送，并作为第二个参数传递，它有三个重要的成员函数，用于返回响应头、响应内容以及结束请求

- res.writeHead(statusCode,[heasers])：向请求的客户端发送响应头，该函数在一个请求中最多调用一次，如果不调用，则会自动生成一个响应头
- res.write(data,[encoding])：想请求的客户端发送相应内容，data是一个buffer或者字符串，如果data是字符串，则需要制定编码方式，默认为utf-8，在res.end调用之前可以多次调用
- res.end([data],[encoding])：结束响应，告知客户端所有发送已经结束，当所有要返回的内容发送完毕时，该函数必需被调用一次，两个可选参数与res.write()相同。如果不调用这个函数，客户端将用于处于等待状态。

<br/>



#### http.request

```
http.request(options,callback)
```

```javascript
var http = require("http");
var querystring = require("querystring");

var postData = querystring.stringify({
    "content": "我真的只是测试一下",
    "mid": 8837
});

var options = {
    hostname: "www.imooc.com",
    port: 80,
    path: "/course/document",
    method: "POST",
    headers: {
        "Accept": "application/json, text/javascript, */*; q=0.01",
        "Accept-Encoding": "gzip, deflate",
        "Accept-Language": "zh-CN,zh;q=0.8",
        "Connection": "keep-alive",
        "Content-Length": postData.length,
        "Content-Type": "application/x-www-form-urlencoded; charset=UTF-8",
        "Host": "www.imooc.com",
        "Origin": "http://www.imooc.com",
        "Referer": "http://www.imooc.com/video/8837",
        "X-Requested-With": "XMLHttpRequest",
    }
}

var req = http.request(options, function(res) {
    res.on("data", function(chunk) {
        console.log(chunk);
    });
    res.on("end", function() {
        console.log("评论完毕！");
    });
    console.log(res.statusCode);
});

req.on("error", function(err) {
    console.log(err.message);
})

req.write(postData);
req.end();
```

<br/>

##### http.ClientRequest

http.ClientRequest是由http.request或者是http.get返回产生的对象，表示一个已经产生而且正在进行中的HTPP请求，提供一个response事件，也就是我们使用http.get和http.request方法中的回调函数所绑定的对象，我们可以显式地绑定这个事件的监听函数。

```javascript
var http = require("http");

var options = {
    hostname: "cn.bing.com",
    port: 80
}

var req = http.request(options);
req.on("response", function(res) {
    res.setEncoding("utf-8");
    res.on("data", function(chunk) {
        console.log(chunk.toString())
    });
    console.log(res.statusCode);
})

req.on("error", function(err) {
    console.log(err.message);
});
req.end();
```

http.ClientRequest也提供了write和end函数，用于向服务器发送请求体，通常用于POST、PUT等操作，**所有写操作都必须调用end函数来通知服务器，否则请求无效**。此外，这个对象还提供了abort()、setTimeout()等方法，具体可以参考文档。

<br/>



##### http.ClientReponse

与http.ServerRequest相似，提供了三个事件，data、end、close，分别在数据到达、传输结束和连接结束时触发，其中data事件传递一个参数chunk，表示接受到的数据。

此外，这个对象提供了几个特殊的函数

- response。setEncoding([encoding])：设置默认的编码，当data事件被触发时，数据将会以encoding编码，默认值是null，也就是不编码，以buffer形式存储
- response.pause()：暂停结束数据和发送事件，方便实现下载功能
- response.resume()：从暂停的状态中恢复

```javascript
var cheerio = require("cheerio");
var http = require("http");
var fs = require("fs");

var options = "http://www.sysu.edu.cn/2012/cn/jgsz/yx/index.htm";
var htmlData = ""
var req = http.request(options, function(res) {
    res.on("data", function(chunk) {
        htmlData += chunk;
    });
    res.on("end", function() {
        var $ = cheerio.load(htmlData);
        var textcontent = $("tr").text();
        fs.writeFile("./school.txt", textcontent, "utf-8")
    });
});
req.end();
```

<br/>

