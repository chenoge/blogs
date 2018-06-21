---
title: Http-form-data
date: 2018-01-30 22:18:43
tags: [form-data]
---

# 一、演变

http协议本身的原始方法不支持`multipart/form-data`请求，是由一些原始的方法演变而来的

1、`multipart/form-data`的**基础方法是post**，也就是说是由`post`方法来组合实现的 

2、`multipart/form-data`与`post`方法的不同之处：**请求头，请求体**。 

3、`multipart/form-data`的请求头必须包含一个特殊的头信息：`Content-Type`，且其值也必须规定为`multipart/form-data` 

4、规定一个**内容分割符**用于分割请求体中的多个post的内容

 

具体的头信息如下：`Content-Type: multipart/form-data; boundary=${bound}`，其中`${bound}` 是一个占位符，代表我们规定的分割符，可以自己任意规定，但为了避免和正常文本重复了，尽量要使用复杂一点的内容。

<br/>

# 二、请求体

`multipart/form-data`的请求体也是一个字符串，不过和post的请求体不同的是它的构造方式，**post是简单的`name=value`值连接**，而`multipart/form-data`则是**添加了分隔符**等内容的构造体。具体格式如下: 

```markdown
--${bound}
Content-Disposition: form-data; name="Filename"
HTTP.pdf
		
		
--${bound}
Content-Disposition: form-data; name="file000"; filename="HTTP协议详解.pdf"
Content-Type: application/octet-stream
		
%PDF-1.5
file content
%%EOF
		
--${bound}
Content-Disposition: form-data; name="Upload"
		
Submit Query
--${bound}--
```

