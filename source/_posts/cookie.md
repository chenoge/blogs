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

| 特性   | Cookie                                 | localStorage                           | sessionStorage                                               |
| ------ | -------------------------------------- | -------------------------------------- | :----------------------------------------------------------- |
| 生命期 | 可设置失效时间，默认是关闭浏览器后失效 | 除非被清除，否则永久保存               | 仅在当前会话下有效，关闭标签或浏览器后被清除                 |
| 大小   | 4K左右                                 | 一般为5MB                              | 一般为5MB                                                    |
| 通信   | 每次都会携带在HTTP头中                 | 仅在客户端中保存，不参与和服务器的通信 | 仅在客户端中保存，不参与和服务器的通信                       |
| 作用域 | 通过`domain、path`来确定的             | 通过`协议、主机名、端口`来确定         | 通过`协议、主机名、端口、标签`来确定。不同标签页面，`sessionStorage`数据是无法共享的 |

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

注：以下可选的**cookie属性值**可以跟在键值对后，用来具体化对cookie的设定/更新，使用分号以作分隔

- `;path=path` 如果没有定义，默认为当前文档位置的路径，例如  `/mydir`
- `;domain=domain` 如果没有定义，默认为当前文档位置的路径的域名部分。例如 `example.com`
  - 如果指定了一个域，那么**子域**也包含在内
- `;max-age=max-age-in-seconds` 例如一年为`60*60*24*365`
- `;expires=date-in-GMTString-format` 如果没有定义，cookie会在对话结束时过期
- `;secure` 只通过`https`协议传输

<br/>



#### 5、cookie匹配规则

浏览器在**加载静态资源**或者**发出ajax请求**时，会根据`资源url`中的**域名与路径**，对每个cookie的`domain属性`和`path属性`进行验证，满足`子串条件`时才会带上

- 存在多个同名cookie时，按一定排序都带上

请求`http://www.example.com/news/info/index.html`时， 带上的cookie要满足：

- `domain属性`是`.www.example.com`的**域名子串**
- `path属性`是`/news/info/index.html`的**路径子串**

注：

- 在浏览器控制台中，`document.cookie`值，对应`index.html`资源的可见cookie值
- cookie的写入是没有限制的。由于读取有限制，会给人一种写入不成功的假象

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