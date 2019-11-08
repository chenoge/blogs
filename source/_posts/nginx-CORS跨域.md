---
title: nginx-CORS跨域
date: 2018-04-10 10:22:29
tags: [nginx,CORS,跨域]
---

# 一、CORS

**CORS**全称`Cross-Origin Resource Sharing`，是HTML5规范定义的如何跨域访问资源。 

**Origin**（**本域**）表示浏览器当前页面的域。当JavaScript向**外域**发起请求后，浏览器收到响应后，首先检查`Access-Control-Allow-Origin`是否包含**本域**，如果是，则此次跨域请求成功，如果不是，则请求失败，JavaScript将无法获取到响应的任何数据。 

![](nginx-CORS跨域\l.png)

上面这种跨域请求，称之为“**简单请求**”。若请求满足所有下述条件，则该请求可视为“简单请求”： 

##### 1.使用下列方法之一

- `GET`
- `HEAD`
- `POST`

##### 2.Fetch 规范定义了对 CORS 安全的首部字段集合，不得人为设置该集合之外的其他首部字段。该集合为

- `Accept`
- `Accept-Language`
- `Content-Language`
- `Content-Type`
- `DPR`
- `Downlink`
- `Save-Data`
- `Viewport-Width`
- `Width`

##### 3.`Content-Type`的值仅限于下列三者之一

- `text/plain`
- `multipart/form-data`
- `application/x-www-form-urlencoded`

##### 4.请求中的任意`XMLHttpRequestUpload`对象均没有注册任何事件监听器

- `XMLHttpRequestUpload`对象可以使用 `XMLHttpRequest.upload`属性访问

##### 5.请求中没有使用 `ReadableStream` 对象

<br/>

<!--more-->

## 二、非简单请求

对于**PUT**、**DELETE**以及其他类型（如`application/json`的POST请求），在发送AJAX请求之前，浏览器会先发送一个`OPTIONS`请求（称为`preflighted`请求）到这个URL上，询问目标服务器是否接受： 

```nginx
OPTIONS /path/to/resource HTTP/1.1
Host: bar.com
Origin: http://my.com
Access-Control-Request-Method: POST
```

服务器必须**响应并明确指出允许的Method**： 

```nginx
HTTP/1.1 200 OK
Access-Control-Allow-Origin: http://my.com
Access-Control-Allow-Methods: POST, GET, PUT, OPTIONS
Access-Control-Max-Age: 86400
```

浏览器确认服务器响应的`Access-Control-Allow-Methods`头确实包含将要发送的AJAX请求的Method，才会继续发送AJAX，否则，抛出一个错误。 

<br/>

# 三、Access-Control- 参数

##### （1）Access-Control-Allow-Origin

该字段是必须的。它的值要么是请求时`Origin`字段的值，要么是一个`*`，表示接受任意域名的请求。

<br/>

##### （2）Access-Control-Allow-Credentials

该字段可选。它的值是一个布尔值，表示是否允许发送Cookie。**默认情况下，Cookie不包括在CORS请求之中**。设为`true`，即表示服务器明确许可，Cookie可以包含在请求中，一起发给服务器。这个值也只能设为`true`，如果服务器不要浏览器发送Cookie，删除该字段即可。

<br/>

##### （2.1）withCredentials 属性

如果要把Cookie发到服务器，一方面要**服务器同意**，指定`Access-Control-Allow-Credentials`字段；另一方面，开发者必须在`AJAX`请求中打开`withCredentials`属性。 

```javascript
let xhr = new XMLHttpRequest();
xhr.withCredentials = true;
```

否则，即使服务器同意发送Cookie，浏览器也不会发送。或者，服务器要求设置Cookie，浏览器也不会处理。 

但是，如果省略`withCredentials`设置，有的浏览器还是会一起发送Cookie。这时，可以显式关闭`withCredentials`。 

注：如果要发送Cookie，`Access-Control-Allow-Origin`就不能设为星号，必须指定明确的、与请求网页一致的域名。同时，Cookie依然遵循同源政策，只有用服务器域名设置的Cookie才会上传，其他域名的Cookie并不会上传，且（跨源）原网页代码中的`document.cookie`也无法读取服务器域名下的Cookie。 

<br/>

##### （3）Access-Control-Expose-Headers

该字段可选。CORS请求时，`XMLHttpRequest`对象的`getResponseHeader()`方法只能拿到6个基本字段：`Cache-Control`、`Content-Language`、`Content-Type`、`Expires`、`Last-Modified`、`Pragma`。如果想拿到其他字段，就必须在`Access-Control-Expose-Headers`里面指定。

<br/>

# 四、什么情况下存在跨域问题

```
1、由 XMLHttpRequest 或 Fetch 发起的跨域 HTTP 请求

2、Web 字体 ，CSS 中通过 @font-face 使用跨域字体资源

3、WebGL 贴图

4、使用 drawImage 将 Images/video 画面绘制到 canvas

5、样式表，使用 CSSOM

```

<br/>

# 五、nginx配置例子

```nginx 
location / {
     if ($request_method = 'OPTIONS') {
        add_header 'Access-Control-Allow-Origin' '*';
        add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
        #
        # Custom headers and headers various browsers *should* be OK with but aren't
        #
        add_header 'Access-Control-Allow-Headers' 'User-Agent,Cache-Control,Content-Type';
        #
        # Tell client that this pre-flight info is valid for 20 days
        #
        add_header 'Access-Control-Max-Age' 1728000;
        add_header 'Content-Type' 'text/plain; charset=utf-8';
        add_header 'Content-Length' 0;
        return 204;
     }
    
     if ($request_method = 'POST') {
        add_header 'Access-Control-Allow-Origin' '*';
        add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
        add_header 'Access-Control-Allow-Headers' 'User-Agent,Cache-Control,Content-Type';
        add_header 'Access-Control-Expose-Headers' 'Content-Length,Content-Range';
     }
    
     if ($request_method = 'GET') {
        add_header 'Access-Control-Allow-Origin' '*';
        add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
        add_header 'Access-Control-Allow-Headers' 'User-Agent,Cache-Control,Content-Type';
        add_header 'Access-Control-Expose-Headers' 'Content-Length,Content-Range';
     }
}
```





