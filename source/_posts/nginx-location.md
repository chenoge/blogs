---
title: nginx-location
date: 2017-09-11 15:37:34
tags: [nginx,location]
---

# 一、location匹配顺序 

1. **"="前缀指令**匹配，如果匹配成功，则停止其他匹配。
2. **普通字符串指令**匹配，顺序是<font color=#A52A2A size=4 >**从长到短**</font>，匹配成功的location如果使用<font color=#A52A2A size=4 >**^~**</font>，则停止其他匹配（正则匹配）。
3. **正则表达式指令**匹配，按照配置文件里的顺序，成功就停止其他匹配。
4. **如果第三步中有匹配成功，则使用该结果，否则使用第二步结果。**



- 匹配的顺序是<font color=#A52A2A size=4 >**先匹配普通字符串，然后再匹配正则表达式**</font>。另外<font color=#A52A2A size=4 >**普通字符串匹配顺序是根据配置中字符长度从长到短**</font>，也就是说使用普通字符串配置的location顺序是无关紧要的，反正最后nginx会根据配置的长短来进行匹配，但是需要注意的是<font color=#A52A2A size=4 >**正则表达式按照配置文件里的顺序**</font>测试。找到第一个比配的正则表达式将停止搜索。

<br/>

<!--more--> 

# 二、匹配模式及顺序

#### 1、模式

- ~ 表示执行一个正则匹配，区分大小写
- ~* 表示执行一个正则匹配，不区分大小写
- ^~ 表示普通字符匹配。使用前缀匹配。如果匹配成功，则不再匹配其他location。
- = 进行普通字符精确匹配。也就是完全匹配。
- @ 它定义一个命名的 location，使用在内部定向时

#### 2、例子

- location **=      /uri**    =开头表示精确匹配，只有完全匹配上才能生效。
- location **^~ /uri**        ^~      开头对URL路径进行前缀匹配，并且在正则之前。
- location **~      pattern**     ~开头表示区分大小写的正则匹配。
- location **~*      pattern**     ~*开头表示不区分大小写的正则匹配。
- location      **/uri**     不带任何修饰符，也表示前缀匹配，但是在正则匹配之后。
- location      **/**     通用匹配，任何未匹配到其它location的请求都会匹配到，相当于switch中的default。 

```nginx
upstream tornado {
    server 127.0.0.1:8001;
}

server {
    server_name luokr.com;
    return 301 $scheme://www.luokr.com$request_uri;
}

server {
    listen 80;
    server_name www.luokr.com;
    root /var/www/www.luokr.com/V0.3/www;
    index index.html index.htm;
    try_files $uri @tornado;
    
    location @tornado {
        proxy_pass_header Server;
        proxy_set_header Host $http_host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Scheme $scheme;
        proxy_pass http://tornado;
    }
}
```

