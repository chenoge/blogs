---
title: nginx-upstream
date: 2019-08-01 16:04:36
tags: [nginx,upstream]
---

##### 1、轮询(weight)

指定轮询几率，**weight和访问比率成正比，用于后端服务器性能不均的情况**。默认当weight不指定时，各服务器weight相同，每个请求按时间顺序逐一分配到不同的后端服务器。**如果后端服务器down掉，能自动剔除**。

```nginx
upstream bakend {
    server 192.168.1.10 weight=1;
    server 192.168.1.11 weight=2;
}
```

<!--more-->

<br/>



##### 2、ip_hash

每个请求按访问ip的hash结果分配，这样每个访客固定访问一个后端服务器，**可以解决session不能跨服务器的问题**。如果**后端服务器down掉，要手工down掉**。

```nginx
upstream resinserver{
    ip_hash;
    server 192.168.1.10:8080;
    server 192.168.1.11:8080;
}
```

<br/>



##### 3、fair（第三方插件）

按后端服务器的响应时间来分配请求，响应时间短的优先分配。

```nginx
upstream resinserver{
    server 192.168.1.10:8080;
    server 192.168.1.11:8080;
    fair;
}
```

<br/>



##### 4、url_hash（第三方插件）

按访问`url`的`hash`结果来分配请求，使每个url定向到同一个后端服务器，**后端服务器为缓存服务器时比较有效**。在upstream中加入hash语句，hash_method指定使用的hash算法。

```nginx
upstream resinserver{
    server 192.168.1.10:8080;
    server 192.168.1.11:8080;
    hash $request_uri;
    hash_method crc32;
}
```

<br/>



##### 5、负载均衡实例

```nginx
upstream tel_img_stream {
    ip_hash;
    server 192.168.11.68:20201;
    server 192.168.11.69:20201 weight=100 down;
    server 192.168.11.70:20201 weight=100;
    server 192.168.11.71:20201 weight=100 backup;
    server 192.168.11.72:20201 weight=100 max_fails=3 fail_timeout=30s;
}
```

说明：

- `down`：表示当前的server暂时**不参与负载** 。
- `weight`：权重，默认为1，weight越大，负载的权重就越大。
- `max_fails`：失败次数，默认为1。当超过`max_fails`时，返回`proxy_next_upstream` 模块定义的错误。
- `fail_timeout`：如果在 `fail_timeout` 期间后端失败了 `max_fails` 次，那么就将这个后端标识为不可用，在接下来的 `fail_timeout` 期间， NGINX 不会再将请求分配到这个后端。
- `backup`：备用服务器，所有`非backup机器`**宕机或者不可用**的时候，请求backup机器。

<br/>



注：

- Nginx默认**判断失败节点状态**以`connect refuse`和`timeout`状态为准，不以**HTTP错误状态**进行判断失败，除非添加了`proxy_next_upstream`指令设置对`404、502、503、504、500和time out`等错误转到备机处理。

<br/>



##### 6、proxy_next_upstream

`proxy_next_upstream`指令，指定了应将请求**传递到下一个服务器**的情况：

```
Syntax:	proxy_next_upstream error | timeout | invalid_header | http_500 | http_502 | http_503 | http_504 | http_403 | http_404 | http_429 | non_idempotent | off ...;
Default:	proxy_next_upstream error timeout;
Context:	http, server, location
```

```
error:在与服务器建立连接，向其传递请求或读取响应标头时发生错误

timeout:在与服务器建立连接，向其传递请求或读取响应头时发生超时

invalid_header:服务器返回空响应或无效响应

http_500:服务器返回了带有代码500的响应

http_502:服务器返回具有代码502的响应

http_503:服务器返回具有代码503的响应

http_504:服务器返回具有代码504的响应

http_403:服务器返回带有代码403的响应

http_404:服务器返回具有代码404的响应

non_idempotent:通常，如果请求已经被发送到服务器，则具有非幂等方法的请求（POST，LOCK，PATCH）不被传递到下一个服务器，启用此选项明确允许重试此类请求

off:禁用将请求传递到下一个服务器
```

<br/>