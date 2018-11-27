---
title: 几种常见的contentType类型
date: 2018-11-27 20:23:37
tags: [contentType,postman]
---

#### 1、`application/x-www-form-urlencoded`

浏览器的原生` <form>` 表单，如果不设置 `enctype` 属性，那么最终就会以 `application/x-www-form-urlencoded` 方式提交数据。 对应`postman`中的`x-www-form-urlencoded `。

```http
POST http://www.example.com HTTP/1.1
Content-Type: application/x-www-form-urlencoded;charset=utf-8

title=test&sub%5B%5D=1&sub%5B%5D=2&sub%5B%5D=3
```

```javascript
decodeURI('title=test&sub%5B%5D=1&sub%5B%5D=2&sub%5B%5D=3')
// "title=test&sub[]=1&sub[]=2&sub[]=3"
// sub：[1,2,3]
```

注：提交的数据按照 `key1=val1&key2=val2` 的方式进行编码，`key`和`val`都进行了`URL`转码 

<!--more-->

<br/>

#### 2、`multipart/form-data`

常见的 POST 数据提交的方式。我们使用表单**上传文件**时，必须让 `form` 的` enctype` 等于这个值。对应`postman`中的`form-data `。

```html
<form action="/" method="post" enctype="multipart/form-data">
  <input type="text" name="description" value="some text">
  <input type="file" name="myFile">
  <button type="submit">Submit</button>
</form>
```

```http
POST /foo HTTP/1.1
Content-Length: 68137
Content-Type: multipart/form-data; boundary=---------------------------974767299852498929531610575

---------------------------974767299852498929531610575
Content-Disposition: form-data; name="description"

some text
---------------------------974767299852498929531610575
Content-Disposition: form-data; name="myFile"; filename="foo.txt"
Content-Type: text/plain

(content of the uploaded file foo.txt)
---------------------------974767299852498929531610575--
```

<br/>

#### 3、`application/json`

消息主体是序列化后的 JSON 字符串,这个类型越来越多地被大家所使用 。对应`postman`中的`json `。

```http
POST [http://www.example.com](http://www.example.com) HTTP/1.1 
Content-Type: application/json;charset=utf-8 

{"title":"test","sub":[1,2,3]}
```

方便的提交复杂的结构化数据，特别适合 RESTful 的接口 

<br/>

##### 4、`text/xml`

是一种使用` HTTP` 作为传输协议，`XML` 作为编码方式的远程调用规范 。对应`postman`中的`xml `。

```http
POST [http://www.example.com](http://www.example.com) HTTP/1.1 
Content-Type: text/xml 
<!--?xml version="1.0"?--> 
<methodcall> 
    <methodname>examples.getStateName</methodname> 
    <params> 
        <param> 
            <value><i4>41</i4></value> 
    </params> 
</methodcall>
```

<br/>

