---
title: nginx-proxy_redirect
date: 2019-07-23 22:19:28
tags: [Nginx,proxy_redirect]
---

#### proxy_redirect 

```
语法：proxy_redirect [default | off | redirect replacement] 
默认值：proxy_redirect default 
使用字段：http,server,location 
```

<!--more-->

<br/>



##### `proxy_redirect redirect replacement`

如果需要修改从被代理服务器传来的应答头中的`Location`和`Refresh`字段，可以用这个指令设置。 假设被代理服务器返回Location字段为：` http://localhost:8000/two/some/uri/ `。

```nginx
proxy_redirect http://localhost:8000/two/  http://frontend/one/; 
# 这个指令将Location字段重写为http://frontend/one/some/uri/

proxy_redirect http://localhost:8000/two/ /; 
# 在代替的字段中可以不写服务器名，这样就使用服务器的基本名称和端口，即使它来自非80端口。
```

 <br/>



##### `proxy_redirect   default`

如果使用`default`参数，将根据`location`和`proxy_pass`参数的设置来决定。例如下列两个配置等效： 

```nginx
location /one/ {  
    proxy_pass http://upstream:port/two/;  
    proxy_redirect   default;
} 

location /one/ {  
    proxy_pass http://upstream:port/two/;  
    proxy_redirect   http://upstream:port/two/   /one/;
} 
```

<br/>



```nginx
# 在指令中可以使用一些变量： 
proxy_redirect   http://localhost:8000/ http://$host:$server_port/; 

#这个指令有时可以重复： 
proxy_redirect   default;  
proxy_redirect   http://localhost:8000/ /;  
proxy_redirect   http://www.example.com/ /; 

# 参数off将在这个字段中禁止所有的proxy_redirect指令
proxy_redirect   off;  
proxy_redirect   default;  
proxy_redirect   http://localhost:8000/    /;  
proxy_redirect   http://www.example.com/   /; 
```