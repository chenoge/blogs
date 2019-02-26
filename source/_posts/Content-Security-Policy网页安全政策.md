---
title: Content-Security-Policy网页安全政策
date: 2019-02-26 15:59:27
tags: [CSP,Content-Security-Policy,网页安全政策]
---

[原文](http://www.ruanyifeng.com/blog/2016/09/csp.html) 

<br/>

#### 一、Content Security Policy - CSP

`CSP `的实质就是**白名单制度**，开发者明确告诉客户端，哪些外部资源可以**加载**和**执行**，等同于提供白名单。它的**实现和执行全部由浏览器完成**，开发者只需通过以下两种方式进行配置即可：

- 通过 HTTP 头信息的`Content-Security-Policy`的字段 
- 通过网页的`<meta>`标签 

##### HTTP

```
Content-Security-Policy: script-src 'self'; object-src 'none'; style-src cdn.example.org third-party.org; child-src https:
```
##### meta

```html
<meta http-equiv="Content-Security-Policy" content="script-src 'self'; object-src 'none'; style-src cdn.example.org third-party.org; child-src https:">
```

上面代码中，CSP 做了如下配置：

> - script-src：只信任当前域名
> - object-src：不信任任何URL，即不加载任何资源
> - style-src：只信任`cdn.example.org`和`third-party.org`
> - child-src：必须使用HTTPS协议加载
> - 其他资源：没有限制

<!--more-->

<br/>

#### 二、各类资源限制

```
script-src：外部脚本
style-src：样式表
img-src：图像
media-src：媒体文件（音频和视频）
font-src：字体文件
object-src：插件（比如 Flash）
child-src：框架
frame-ancestors：嵌入的外部资源（比如<frame>、<iframe>、<embed>和<applet>）
connect-src：HTTP 连接（通过 XHR、WebSockets、EventSource等）
worker-src：worker脚本
manifest-src：manifest 文件
default-src：用来设置上面各个选项的默认值 
```

```
Content-Security-Policy: default-src 'self'
# 限制所有的外部资源，都只能从当前域名加载
```

注：如果同时设置某个单项限制和`default-src`，某个单项限制会覆盖`default-src`的值。

<br/>

#### 三、限制值

```
主机名：example.org，https://example.com:443
路径名：example.org/resources/js/
通配符：*.example.org，*://*.example.com:*（表示任意协议、任意子域名、任意端口）
协议名：https:、data:
关键字'self'：当前域名，需要加引号
关键字'none'：禁止加载任何外部资源，需要加引号
```

注：

- 多个值也可以并列，用空格分隔
- 如果同一个限制选项使用多次，只有第一次会生效 

<br/>

#### 四、script-src 的特殊值

```
unsafe-inline：允许执行页面内嵌的&lt;script>标签和事件监听函数
unsafe-eval：允许将字符串当作代码执行，比如使用eval、setTimeout、setInterval和Function等函数。
nonce值：每次HTTP回应给出一个授权token，页面内嵌脚本必须有这个token，才会执行
hash值：列出允许执行的脚本代码的Hash值，页面内嵌脚本的哈希值只有吻合的情况下，才能执行。
```

<br/>

##### `nonce`值例子 

```
# 1、服务器发送网页的时候，告诉浏览器一个随机生成的token
Content-Security-Policy: script-src 'nonce-EDNnf03nceIOfn39fn3e9h3sdfa'

# 2、页面内嵌脚本，必须有这个token才能执行
<script nonce=EDNnf03nceIOfn39fn3e9h3sdfa>// some code</script>
```

<br/>

##### hash值例子 

```
# 1、服务器给出一个允许执行的代码的hash值
Content-Security-Policy: script-src 'sha256-qznLcsROx4GACP2dm0UCKCzCG-HiZ1guq6ZZDob_Tng='

# 2、下面的代码就会允许执行，因为hash值相符
<script>alert('Hello, world.');</script>
```

注：

- 计算hash值的时候，`<script>`标签不算在内
- 除了`script-src`选项，nonce值和hash值还可以用在`style-src`选项，控制页面内嵌的样式表 

<br/>

##### 微信公众号网页的信息

```
Content-Security-Policy: script-src 'self' 'unsafe-inline' 'unsafe-eval' http://*.qq.com https://*.qq.com http://*.weishi.com https://*.weishi.com 'nonce-763410929';style-src 'self' 'unsafe-inline' http://*.qq.com https://*.qq.com;object-src 'self' http://*.qq.com https://*.qq.com;font-src 'self' data: http://*.qq.com https://*.qq.com http://fonts.gstatic.com https://fonts.gstatic.com;frame-ancestors 'self' http://wx.qq.com https://wx.qq.com http://wx2.qq.com https://wx2.qq.com  http://wx8.qq.com https://wx8.qq.com http://web.wechat.com https://web.wechat.com http://web1.wechat.com https://web1.wechat.com http://web2.wechat.com https://web2.wechat.com http://sticker.weixin.qq.com https://sticker.weixin.qq.com http://bang.qq.com https://bang.qq.com http://app.work.weixin.qq.com https://app.work.weixin.qq.com http://work.weixin.qq.com https://work.weixin.qq.com http://finance.qq.com https://finance.qq.com http://gu.qq.com https://gu.qq.com http://wzq.tenpay.com https://wzq.tenpay.com;report-uri https://mp.weixin.qq.com/mp/fereport?action=csp_report
```

<br/>