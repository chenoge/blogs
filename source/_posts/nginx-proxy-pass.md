---
title: nginx-proxy_pass
date: 2017-11-16 16:13:19
tags: [nginx,proxy_pass]
---

# 一、路径问题

- **当proxy_pass后有url时，则nginx不会把location中匹配的路径部分代理走**
- **当proxy_pass后没有url时，则会把匹配的路径部分也给代理走**

```nginx
location ^~ /static_js/ 
{ 
    proxy_cache js_cache; 
    proxy_set_header Host js.test.com; 
    proxy_pass http://js.test.com/; 
}
```

如上面的配置，如果请求的url是 **/static_js/test.html** ，会被代理成 **test.html**。

```nginx
location ^~ /static_js/ 
{ 
    proxy_cache js_cache; 
    proxy_set_header Host js.test.com; 
    proxy_pass http://js.test.com; 
}
```

则会被代理到 **/static_js/test.htm** 。

<br/>

<!--more--> 

# 二、proxy_set_header

#### 1、原理介绍

```nginx
proxy_set_header field value;
```

| 语法:   | **proxy_set_header** field value;                            |
| ------- | ------------------------------------------------------------ |
| 默认值: | proxy_set_header  **Connection**  close; <br />proxy_set_header  **Host**  $proxy_host; |
| 上下文: | http, server, location                                 |

**允许重新定义或者添加发往后端服务器的<font color=#A52A2A size=4 >请求头**</font>。`value`可以包含文本、变量或者它们的组合。 当且仅当当前配置级别中没有定义`proxy_set_header`指令时，会从上面的级别继承配置。 默认情况下，只有两个请求头会被重新定义： 

```nginx 
proxy_set_header Host $proxy_host;
proxy_set_header Connection close;
```

proxy_set_header也可以**自定义参数**，如：

```nginx
proxy_set_header test paroxy_test; 
```

如果想要支持下划线的话，需要在http或者server中 增加如下配置： 

```nginx 
underscores_in_headers on|off
```

#### 2、获取用户ip

在实际应用中，我们可能需要获取**用户的ip地址。**通常情况下我们使用request.getRemoteAddr()就可以获取到客户端ip，但是当我们使用了nginx作为反向代理后，使用request.getRemoteAddr()获取到的就一直是nginx服务器的ip的地址。

- 方案1：X-real-ip

  - nginx是可以获得用户的真实ip的，也就是说nginx使用$remote_addr变量时获得的是用户的真实ip，如果我们想要在**服务器端**获得用户的真实ip，就必须在nginx这里作一个赋值操作，如下： 

  ```nginx
  # X-real-ip是一个自定义的变量名
  proxy_set_header X-real-ip $remote_addr;
  
  # 在服务器端可以这样获取
  # request.getAttribute("X-real-ip")
  ```

<br/>

- 方案2：X-Forwarded-For 

  - 一般来说，`X-Forwarded-For`是用于记录代理信息的，**每经过一级代理(匿名代理除外)，代理服务器都会把这次请求的`来源IP`追加在`X-Forwarded-For`中** 。

  ```nginx
  # 192.168.107.107 nginx.conf
  location /test {
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_pass http://192.168.107.112:8080;
  }
  
  
  #192.168.107.112 nginx.conf
  location /test {
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_pass http://192.168.107.114:8080;
  }
  
  
  #192.168.107.114 nginx.conf
  location /test {
      default_type text/html;
      charset gbk;
      echo "$remote_addr ||$http_x_real_ip  ||$http_x_forwarded_for";
  }
  
  # 192.168.107.112 || 192.168.162.16 || 192.168.162.16, 192.168.107.107
  ```

  

#### 3、实战应用

从微信小程序跳到H5页面，需要在后台配置相关的**业务域名**。当我们需要跳到其他网站的页面时，可以使用Nginx来做反代理配置。如：在微信后台配置 **es.yf-gz.cn**，反代理到 **shop.mallparking.cn**

```nginx
server {
    listen 443 ssl;
    server_name es.yf-gz.cn;
    ssl on;
    ssl_certificate /etc/nginx/cer/1_es.yf-gz.cn.pem;
    ssl_certificate_key /etc/nginx/cer/2_es.yf-gz.cn.key;
    ssl_session_timeout 5m;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers AESGCM:ALL:!DH:!EXPORT:!RC4:+HIGH:!MEDIUM:!LOW:!aNULL:!eNULL;
    ssl_prefer_server_ciphers on;

   location / {
       proxy_pass http://www.mallparking.cn;
       proxy_redirect off;
       proxy_set_header Host www.mallparking.cn;
       proxy_set_header X-Real_IP $remote_addr;
       proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
       proxy_set_header X-Forwarded-Proto https;
  }
}
```

