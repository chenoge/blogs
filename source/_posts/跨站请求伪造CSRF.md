---
title: 跨站请求伪造CSRF
date: 2018-11-26 21:09:12
tags: [CSRF,攻击]
---

### 一、CSRF攻击原理

![](跨站请求伪造CSRF\2009040916453171.jpg)



从上图可以看出，要完成一次`CSRF`攻击，受害者必须依次完成两个步骤：

　　1.登录受信任网站A，并在本地生成Cookie

　　2.在不登出A的情况下，访问危险网站B

注：

- 如果**不满足以上两个条件中的一个**，就不会受到`CSRF`的攻击
- **网站B**向**网站A**发送请求时，如果网站A在**打开**状态下，那么**该请求**会携带**网站A页面**下的`cookie`（待验证）
- `CSRF`攻击是以**突破同源策略**限制为前提的

<!--more-->

<br/>

### 二、示例

银行网站A：它以GET请求来完成银行转账的操作，如：

```javascript
http://www.mybank.com/Transfer.php?toBankId=11&money=1000
// 使用get来更改资源，真是作死！！！
```

危险网站B：它里面有一段HTML的代码如下：

```html
　<img src=http://www.mybank.com/Transfer.php?toBankId=11&money=1000>
```

首先，你登录了银行网站A，然后访问危险网站B，噢，这时你会发现你的银行账户少了1000块...... 

<br/>

### 三、CSRF防御

#### 1、Cookie Hashing 

所有表单中，生成一个隐藏域 ，它的值就是`Cookie `中的`token` 。

注：`CSRF`能模拟请求，不能跨域读取`cookie`。如果`cookie`已经被`xss`攻击，还是很危险。

<br/>

#### 2、检查Referer字段 

HTTP头中有一个Referer字段，这个字段用以标明请求来源于哪个地址。在处理敏感数据请求时，通常来说，Referer字段应和请求的地址位于同一域名下。 

注：http协议对此字段的内容有明确的规定，但并无法**保证来访的浏览器的具体实现**，亦无法保证浏览器没有安全漏洞影响到此字段。并且也存在攻击者攻击某些浏览器，**篡改**其Referer字段的可能。 


<br/>

#### 3、使用JSON格式

使用`JavaScript`发起`AJAX`请求是限制跨域的，并不能通过简单的 `<form>` 表单来发送`JSON`，所以，通过只接收`JSON`可以很大可能避免`CSRF`攻击。 

注：[`CORS`](https://chenoge.github.io/2018/04/10/nginx-CORS%E8%B7%A8%E5%9F%9F/)规范中对跨域访问资源规定了明确的限制

<br/>









　