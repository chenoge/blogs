---
title: url长度限制
date: 2018-12-29 14:32:04
tags: [url,长度限制]
---

#### 一、HTTP协议

`HTTP协议`不对`url`的长度设置任何先验限制

- 服务器必须能够处理它们所服务的`任何资源的URI`，并且**如果它们提供可以生成这种`URI`的基于`GET`的表单**，则应该能够处理`无限长度的URI`。
- 如果`URI`长于服务器可以处理的长度，服务器应该返回`414`（`Request-URI` Too Long）状态 。

<br/>

<!--more-->

#### 二、浏览器

**1、IE**：`IE浏览器`对`url`长度限制是`2083（2K+53）`字符，超过这个限制，则自动截断

- 若是form提交则提交按钮不起作用

注：实测超过`2048`字符会截断（2018-12-3）

<br/>

**2、firefox**：`firefox`浏览器对`url`长度限制为 `65,536`字符

- 实际上有效的`URL`最大长度不少于`100,000`个字符

<br/>

**3、chrome**：`chrome`浏览器对`url`长度限制为`8182字符`

<br/>

**4、Safari**：`Safari`浏览器对`url`长度限制至少为` 80,000 `字符

<br/>

**5、Opera**：`Opera` 浏览器对`url`长度限制为`190 000 `字符

 <br/>

注： 

- `URL`只能使用英文字母、阿拉伯数字和某些标点符号。不能使用其他文字和符号，**否则必须编码后使用** 

- 即使是`2048`个`ASCII`字符也能满足大多数的使用场景，可以放心使用。

<br/>

#### 三、服务器

**1、Apache**：`Apache`能接受`url长度`限制为`8,192` 字符

**2、ngnix**：可以通过修改配置来改变`url长度`限制

```nginx
client_header_buffer_size 1k # 默认值
large_client_header_buffers 4 4k/8k # 默认值
```

<br/>

#### 四、URL编码

[原文](http://www.ruanyifeng.com/blog/2010/02/url_encoding.html) 

- 网页路径的编码，用的是utf-8编码

```
http://zh.wikipedia.org/wiki/春节
```

- 查询字符串的编码，用的是操作系统的默认编码

```
http://www.baidu.com/s?wd=春节
```

- GET和POST方法的编码，用的是网页的编码

```
// 在已打开的网页上，直接用Get或Post方法发出HTTP请求
<meta http-equiv="Content-Type" content="text/html;charset=xxxx">
```

- 在Ajax调用中，IE总是采用GB2312编码（操作系统的默认编码），而Firefox总是采用utf-8编码

```
三种情况都是由浏览器发出HTTP请求，最后一种情况则是由Javascript生成HTTP请求，也就是Ajax调用
```

注：文章太久远了，可能都默认`utf-8`了（猜测，未验证`2018-12-30`）

<br/>



 

 

 

 

  

 

 