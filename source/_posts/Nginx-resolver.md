---
title: Nginx-resolver
date: 2019-07-26 20:21:08
tags: [resolver]
---

##### `resolver`语法

```
Syntax:    resolver address ... [valid=time] [ipv6=on|off];
Default:   —
Context:   http, server, location
```

- 可以配置多个dns服务，nginx会采用轮询的方式去访问dns服务

- nginx会缓存dns对域名解析的结果，缓存的时间由`valid`指定
- ipv6用于显示开启或者关闭ipv6

<!--more-->

<br/>



##### `resolver_timeout`语法

```
Syntax:  resolver_timeout time;
Default: resolver_timeout 30s;
Context: http, server, location
```

注：`resolver_timeout`用于指定dns解析的超时时间

<br/>

