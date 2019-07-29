---
title: nginx常见错误
date: 2019-07-19 22:53:01
tags: [nginx]
---

##### 1、配置通用代理

```nginx
# url=https://img1.tuicool.com/63MBryU.jpg!web
# 代理url，不包含参数
http {
    resolver 223.5.5.5 119.29.29.29 8.8.8.8;
    server {
        location ~ /api/img/proxy {
            if ($arg_url ~* ^(.*://)?([^/]+)(/.+)) {
                set $domain $2;
            }
            proxy_set_header Host $domain;
            proxy_pass $arg_url;
        }
    }
}
```

```nginx
# url=https://webquoteklinepic.eastmoney.com/GetPic.aspx?nid=116.01117&imageType=k
# 代理url，包含参数
http {
    resolver 223.5.5.5 119.29.29.29 8.8.8.8;
    server {
        location ~ /api/img/proxy {
            if ($query_string ~* ^url=(.*)$) {
                set $proxy_url $1;
            }
            if ($proxy_url ~* ^(.*://)?([^/]+)(/.+)) {
                set $proxy_domain $2;
            }
            add_header cache-control public;
            proxy_set_header Referer $proxy_url;
            proxy_set_header Host $proxy_domain;
            proxy_pass $proxy_url;
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