---
title: nginx-limit限速
date: 2018-06-27 21:27:03
tags: [nginx,limit_conn_zone,limit_conn,limit_rate_after,limit_rate,限速]
---

# 一、**limit_conn** 

#### 语法

```nginx
Syntax:	limit_conn zone number;
Context:	http, server, location
# zone 特定键值
# number 最大允许连接数
```

设置`特定键值`的`共享内存区域`和`最大允许连接数`。超过此限制时，服务器将返回错误以回复请求。

#### 配置

```nginx
http {
    limit_conn_zone $binary_remote_addr zone=perip:10m;
    limit_conn_zone $server_name zone=perserver:10m;
    limit_conn_zone $server_name zone=peradd:10m;
    
    ...
        
    server {
        limit_conn perip 10;
        limit_conn perserver 100;

        ...

        location /download/ {
            limit_conn peradd 1;
        }

# perip 、perserver 和 peradd  是自定义的特定键值
# 分别标志着某个共享内存区域
```

<br/>

<!--more-->

# 二、limit_conn_zone

#### 语法 

```nginx 
Syntax:	limit_conn_zone key zone=name:size;
Context:	http
# size共享内存区域大小
```

#### key

为`共享内存区域`设置参数，该区域将保留`各种键`的状态。特别是，该状态包含当前的`连接数`。`key`可以包含文本，变量及其组合。在这里，客户端IP地址作为`key`。 

#### size

一个`1M`的区域可以保持大约32000个`ipv4`的状态或大约16000个`ipv6`的状态。如果区域存储耗尽，服务器会将错误返回给所有其他请求。 

<br/>

# 三、**limit_rate** 

#### 语法

```nginx
Syntax:	limit_rate rate;
Default:	limit_rate 0;
Context:	http, server, location, if in location
# 限制对客户的响应传输速率
```

- 零值禁用速率限制

- 限制是根据请求设置的，所以如果客户端同时打开两个连接，总体速率将是指定限制的两倍 

- 速率限制也可以在`$limit_rate`变量中设置 

  - ```nginx
    server {
    
        if ($slow) {
            set $limit_rate 4k;
        }
    
        ...
    }
    ```

<br/>

# 四、**limit_rate_after** 

#### 语法

```nginx 
Syntax:	limit_rate_after size;
Default:	limit_rate_after 0;
Context:	http, server, location, if in location
```

设置`初始流量`，**超过设置的流量后**将对客户进一步传送将受到速率限制。 

```nginx 
location /flv/ {
    limit_rate_after 500k;
    limit_rate       50k;
}
```

