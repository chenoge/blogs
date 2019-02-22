---
title: HTTP-Content-Disposition
date: 2019-02-22 19:20:37
tags: [http,Content-Disposition]
---

在常规的HTTP应答中，`Content-Disposition` 消息头指示回复的内容该以何种形式展示，是以**内联**的形式（即网页或者页面的一部分），还是以**附件**的形式下载并保存到本地。 

##### 1、作为消息主体中的消息头

在HTTP场景中，第一个参数或者是`inline`（默认值，表示回复中的消息体会以页面的一部分或者整个页面的形式展示），或者是`attachment`（意味着消息体应该被下载到本地；大多数浏览器会呈现一个“保存为”的对话框，将`filename`的值预填为下载后的文件名，假如它存在的话）。

```http
Content-Disposition: inline
Content-Disposition: attachment
Content-Disposition: attachment; filename="filename.jpg"
```

<br/>

##### 2、作为multipart body中的消息头

在HTTP场景中。第一个参数总是固定不变的`form-data`；附加的参数不区分大小写，并且拥有参数值，参数名与参数值用等号(=)连接，参数值用双引号括起来。参数之间用分号(;)分隔。

```http
Content-Disposition: form-data
Content-Disposition: form-data; name="fieldName"
Content-Disposition: form-data; name="fieldName"; filename="filename.jpg"
```

<!--more-->

<br/>

##### 3、示例

以下是一则可以触发"保存为"对话框的服务器应答：

```http
200 OK
Content-Type: text/html; charset=utf-8
Content-Disposition: attachment; filename="cool.html"
Content-Length: 22

<HTML>Save me!</HTML>
```

这个简单的HTML文件会被下载到本地而不是在浏览器中展示。

大多数浏览器默认会建议将`cool.html`作为文件名。

<br/>

以下是一个HTML表单的示例，展示了在 `multipart/form-data` format 格式的报文中使用`Content-Disposition` 消息头的情况：

```http
POST /test.html HTTP/1.1
Host: example.org
Content-Type: multipart/form-data;boundary="boundary"

--boundary
Content-Disposition: form-data; name="field1"

value1
--boundary
Content-Disposition: form-data; name="field2"; filename="example.txt"

value2
--boundary--
```

<br/>