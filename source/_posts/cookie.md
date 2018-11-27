---
title: Cookie 和 Referer
date: 2018-11-26 20:00:34
tags: [Cookie,XSS,CSRF]
---

### 一、Cookie 

#### 1、`Secure` 和`HttpOnly` 标记

-  `Secure` 

标记为 `Secure` 的`Cookie`只应通过被`HTTPS`协议加密过的请求发送给服务端。但即便设置了 `Secure` 标记，敏感信息也不应该通过`Cookie`传输，因为`Cookie`有其固有的不安全性，`Secure `标记也无法提供确实的安全保障。从 Chrome 52 和 Firefox 52 开始，不安全的站点（`http:`）无法使用Cookie的 `Secure` 标记。

<br/>

- `HttpOnly` 

为避免跨域脚本 `XSS` ，API无法访问带有 `HttpOnly` 标记的Cookie，它们只应该发送给服务端。如果包含服务端 `Session` 信息的 `Cookie` 不想被客户端` JavaScript` 脚本调用，那么就应该为其设置 `HttpOnly` 标记。

<!--more-->

<br/>



#### 2、`SameSite` Cookies

`SameSite` Cookie允许服务器要求某个cookie在**跨站请求**时不会被发送，从而可以阻止跨站请求伪造攻击`CSRF`。但目前`SameSite` Cookie还处于**实验阶段**，并不是所有浏览器都支持。 

<br/>



#### 3、跟localStorage、sessionStorage 相比

| 特性   | Cookie                                     | localStorage                                           | sessionStorage                                               |
| ------ | ------------------------------------------ | ------------------------------------------------------ | :----------------------------------------------------------- |
| 生命期 | 可设置失效时间，默认是关闭浏览器后失效     | 除非被清除，否则永久保存                               | 仅在当前会话下有效，关闭标签或浏览器后被清除                 |
| 大小   | 4K左右                                     | 一般为5MB                                              | 一般为5MB                                                    |
| 通信   | 每次都会携带在HTTP头中                     | 仅在客户端中保存，不参与和服务器的通信                 | 仅在客户端中保存，不参与和服务器的通信                       |
| 作用域 | 通过`domain`文档源和`path`文档路径来确定的 | `文档源`级别，通过`协议`、`主机名`以及`端口`三者来确定 | 通过`文档源`和`标签`来确定。相同文档源，但不同标签的页面，sessionStorage数据是无法共享的 |

<br/>

### 二、Referer

`Referer` 表示**请求页面**是通过**此来源页面**里的链接进入的，在以下两种情况下，`Referer` 不会被发送：

- **来源页面**采用的协议为表示**本地文件**的 "file" 或者 "data" URI
- 当前**请求页面**采用的是非安全协议，而**来源页面**采用的是安全协议（HTTPS）

注：

- 在浏览器中，`Referer` 属性只读不写。但可以通过代理服务进行修改
- 如果需要通过 document.referrer 采集页面访问来源，最好不要使用 JS 跳转或打开新窗口，也不要使用 meta 跳转

<br/>