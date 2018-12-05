---
title: BASIC_HTTP认证_nginx
date: 2018-12-05 16:20:11
tags: [HTTP,BASIC,authentication,nginx_http_auth_basic_module]
---

### 一、BASIC认证概述

HTTP协议定义了**基本认证过程**，允许HTTP服务器对WEB浏览器进行用户身份证的方法。

1. 当一个客户端向HTTP服务器进行数据请求时，如果客户端未被认证，则HTTP服务器将通过基本认证过程对客户端的用户名及密码进行验证，以决定用户是否合法。
2. 客户端在接收到HTTP服务器的身份认证要求后，会提示用户输入用户名及密码，然后将用户名及密码以**BASE64加密**，加密后的密文将附加于请求信息中， 如当用户名为anjuta，密码为：123456时，客户端将用户名和密码**用“`：`”合并**，并将合并后的字符串用BASE64加密为密文，并于每次请求数据 时，将密文附加于请求头（Request Header）中。
3. HTTP服务器在**每次收到请求包**后，根据协议取得客户端附加的用户信息（BASE64加密的用户名和密码），解开请求包，对用户名及密码进行验证，如果用 户名及密码正确，则根据客户端请求，返回客户端所需要的数据;否则，返回错误代码或重新要求客户端提供用户名及密码。

<br/>

<!--more-->

### 二、BASIC认证的过程

1、客户端向服务器请求数据，请求的内容可能是一个网页或者是一个其它的MIME类型，此时，假设客户端尚未被验证(即header中不带正确的Authorization字段)，则客户端提供如下请求至服务器:

```http
Get /index.html HTTP/1.0
Host:www.google.com
```

<br/>

2、  服务器向客户端发送验证请求代码401,服务器返回的数据大抵如下： 

```http
HTTP/1.0 401 Unauthorised
Server: SokEvo/1.0
WWW-Authenticate: Basic realm="google.com"
Content-Type: text/html
Content-Length: xxx
```

<br/>

3、当符合http规范的客户端收到`401`返回值时，将自动弹出一个登录窗口，要求用户输入用户名和密码。 

<br/>

4、用户输入用户名和密码后，将用户名及密码以`BASE64`加密方式加密，并将密文放入前一条请求信息中，则客户端发送的第一条请求信息则变成如下内容(即在header中自动加入`Authorization`字段)： 

```http
Get /index.html HTTP/1.0
Host:www.google.com
Authorization: Basic xxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

<br/>

5、服务器收到上述请求信息后，将`Authorization`字段后的用户信息取出、解密，将解密后的用户名及密码与用户数据库进行比较验证，如用户名及密码正确，服务器则根据请求，将所请求资源发送给客户端。

<br/>

### 三、nginx用户认证配置

`nginx_http_auth_basic_module`模块实现让访问着，只有输入正确的用户密码才允许访问web内容。默认情况下nginx已经安装了`ngx_http_auth_basic_module`模块，如果不需要这个模块，可以加上` --without-http_auth_basic_module `。

##### 1、nginx basic auth指令

```
语法:       auth_basic string | off;
默认值:     auth_basic off;
配置段:     http, server, location, limit_except
```

默认表示不开启认证，后面如果跟上字符，这些字符会在弹窗中显示。 

```
语法:       auth_basic_user_file file;
默认值:     —
配置段:     http, server, location, limit_except
```

生成密码可以使用htpasswd，或者使用openssl。用户密码文件，文件内容类似如下： 

```
# printf "ttlsa:$(openssl passwd -crypt 123456)\n" >>conf/htpasswd
# cat conf/htpasswd
ttlsauser1:password1
```

```nginx
server {
    server_name www.ttlsa.comttlsa.com;
    index index.html index.php;
    root /data/site/www.ttlsa.com;
    
    location / {
        auth_basic "nginx basic http test for ttlsa.com";
        auth_basic_user_file conf/htpasswd;
    }
}
```

