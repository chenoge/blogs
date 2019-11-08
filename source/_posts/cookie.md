---
title: Cookie 和 Referer
date: 2018-11-26 20:00:34
tags: [Cookie,XSS,CSRF]
---

### 一、Cookie

#### 1、`Secure` 和`HttpOnly` 标记

-  `Secure` ：标记为 `Secure` 的`Cookie`，只应通过被`HTTPS`协议加密过的请求发送给服务端

- `HttpOnly` ：标记为 `HttpOnly` 的`Cookie`，` JavaScript` 无法访问带有 `HttpOnly` 标记的`Cookie`，它们只应该发送给服务端，避免跨域脚本 `XSS` 



#### 2、`SameSite` Cookies

`SameSite` Cookie允许服务器要求某个cookie在**跨站请求**时不会被发送，从而可以阻止跨站请求伪造攻击`CSRF`。但目前`SameSite` Cookie还处于**实验阶段**，并不是所有浏览器都支持。 

<!--more-->



#### 3、跟localStorage、sessionStorage 相比

| 特性   | Cookie                                                       | localStorage                           | sessionStorage                                               |
| ------ | ------------------------------------------------------------ | -------------------------------------- | :----------------------------------------------------------- |
| 生命期 | 可设置失效时间，默认为`session cookie`；`session cookie`无法保证**会话**结束后，一定会被删除 | 除非被清除，否则永久保存               | 仅在当前会话下有效，关闭会话（标签页）后被清除；如果标签页由其他页面打开，当前的`sessionStorage`会根据前一个页面的`sessionStorage`数据进行**初始化** |
| 大小   | 4K左右                                                       | 一般为5MB                              | 一般为5MB                                                    |
| 通信   | 每次都会携带在HTTP头中                                       | 仅在客户端中保存，不参与和服务器的通信 | 仅在客户端中保存，不参与和服务器的通信                       |
| 作用域 | 通过`domain、path`来确定                                     | 通过`协议、主机名、端口`来确定         | 通过`协议、主机名、端口、标签`来确定。不同标签页面，`sessionStorage`数据无法共享 |

注：`cookie`通过【`name,doamin,path`】来确定其唯一性，倘若三个属性都一样方为同一个`cookie`

<br/>



#### 4、读写cookie

```js
// 读取所有可从此位置访问的Cookie
let allCookies = document.cookie;

// 一次只能对一个cookie进行设置或更新
// newCookie是一个键值对形式的字符串
document.cookie = newCookie;
```

注：以下可选的**cookie属性值**可以跟在键值对后，用来具体化对cookie的**设定/更新**，**使用分号以作分隔**。在浏览器控制台中，`document.cookie`值，对应**当前资源**的可见cookie值。

- `;path=path` 如果没有定义，默认为当前文档位置的路径
- `;domain=domain` 如果没有定义，默认为当前文档位置的路径的域名部分
- `;max-age=max-age-in-seconds`
- `;expires=date-in-GMTString-format` 如果没有定义，cookie会在对话结束时过期
- `;secure` 只通过`https`协议传输

<br/>



#### 5、cookie匹配规则

浏览器在**加载静态资源**或者**发出ajax请求**时，会根据`url`中的**域名与路径**，对每个cookie的`domain属性`和`path属性`进行验证，存在多个同名cookie时，按一定排序都带上。

请求`http://news.example.com/news/hot`时， 带上的cookie要满足：

- `domain属性`是`news.example.com`，及其**上级域名**
- `path属性`是`/news/hot`，及其**上级路径**

<br/>



#### 6、cookie排序原则

- 具有**更长path**的cookie更靠前
- 如果path长度相等，**更早创建**的cookie更靠前

<br/>



### 二、Referer

`Referer` 表示**请求页面**是通过**此来源页面**里的链接进入的，在以下两种情况下，`Referer` 不会被发送：

- **来源页面**采用的协议为表示**本地文件**的 "file" 或者 "data" URI
- 当前**请求页面**采用的是非安全协议，而**来源页面**采用的是安全协议（HTTPS）

注：

- 在浏览器中，`Referer` 属性只读不写。但可以通过代理服务进行修改
- 如果需要通过 document.referrer 采集页面访问来源，最好不要使用 JS 跳转或打开新窗口，也不要使用 meta 跳转

<br/>