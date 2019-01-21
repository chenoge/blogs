---
title: Nginx配置Host字段出错
date: 2019-01-21 19:41:18
tags: [nginx,Host,http]
---

#### 一、Host为空

```nginx
# 虚拟主机的主域名
proxy_set_header Host       $proxy_host;
```

```nginx
# 请求头中'Host'的值
# 如果请求头中没有携带Host值，那传到服务器的请求也不含这个字段
proxy_set_header Host       $http_host;
```

```nginx
# 请求包含'Host'请求头时为'Host'字段的值
# 在请求未携带'Host'请求头时为虚拟主机的主域名
proxy_set_header Host       $host;
```

注：使用`$http_host`变量时，在`http1.1`协议中，如果请求不带`Host`字段值，会报错`400`错误

- 客户端必须在所有`HTTP1.1`请求消息中包含`Host`头字段
- 如果请求的`URI`不包含所请求服务的主机名，则必须为`Host`头字段指定一个空值
- ` HTTP1.1`代理必须确保它转发的任何请求消息，都包含一个适当的主机头字段，用于标识代理请求的服务
- 所有基于`HTTP1.1`服务器必须以`400`状态代码响应任何缺少主机头字段的`HTTP1.1`请求消息

<!--more-->

<br/>

#### 二、Host重复配置

```nginx
# proxy.conf文件
proxy_set_header        Host            $host;
proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
```

```nginx
include conf/proxy.conf; # proxy.conf文件中已经设置了Host字段值
proxy_set_header        Host            $host;
proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
```

注：两个`proxy_set_header        Host            $host;`语句，配置在同一个语句块中，不存在**子语句块**覆盖**父语句块**的情况

<br/>

`HTTP RFC2616`

可以存在**具有相同字段名的多个消息头字段**在消息中，当且仅当该标头的整个字段值字段被定义为以逗号分隔的列表。通过将**每个后续字段值附加到第一个**，每个用逗号分隔。因此**代理不得改变转发时这些字段值的顺序** 。

```nginx
Cache-Control: no-cache
Cache-Control: no-store
# 相当于
Cache-Control: no-cache, no-store
```

由于`Host`字段值**不允许以逗号分隔**，`nginx`会报`400`状态码错误。

<br/>

