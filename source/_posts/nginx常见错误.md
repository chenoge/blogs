---
title: nginx常见错误
date: 2019-07-19 22:53:01
tags: [nginx]
---

##### 1、配置通过图片代理

```nginx
# http://chenoge.github.io/api/img/proxy?url=https://img1.tuicool.com/63MBryU.jpg!web
http {
    resolver 8.8.8.8;
    server {
        server_name  chenoge.github.io;
        location ~ /api/img/proxy {
            if ($arg_url ~* ^(.*://)?(.+\.\w+)(/.+)) {
                set $domain $2;
            }
            proxy_set_header Host $domain;
            proxy_pass $arg_url;
        }
    }
}
```

<!--more-->

<br/>

##### 遇到的坑点

```
问题：nginx: [emerg] unknown directive "if($domain" in /etc/nginx/nginx.conf:38

原因：if 语法后面，必须要有空格
```

```
问题：nginx：“no resolver defined to resolve xxx.xxx”

原因：Nginx0.6.18以后的版本中启用了一个resolver指令，在使用变量来构造某个server地址的时候一定要用resolver指令来制定DNS服务器的地址

方案：在nginx的配置文件中的http{}部分添加一行resolver 8.8.8.8;即可
```

<br/>