---
title: nginx基本使用
date: 2018-11-19 22:30:45
tags: [nginx]
---

### 一、[常用命令](http://nginx.org/en/docs/beginners_guide.html)

```shell
nginx -?,-h #查看帮助
nginx -v #查看版本
nginx #启动服务
service nginx restart #重启服务
nginx -s reload|stop|quit #重载配置、停止服务
```

注：

- reload：重新加载配置文件，Nginx服务不会中断。检查语法，如果出错会rollback
- stop：快速停止 ，不管有没有正在处理的请求 
- quit：正常停止，退出前完成已经接受的连接请求 。只有启动`Nginx`的用户才能执行该命令

<!--more-->

<br/>

### 二、nginx配置

#### 1、配置文件`/etc/nginx/nginx.conf`

#### 2、文件结构

```nginx
...              #全局块

events {         #events块
   ...
}

http      #http块
{
    ...   #http全局块
    server        #server块
    { 
        ...       #server全局块
        location [PATTERN]   #location块
        {
            ...
        }
        location [PATTERN] 
        {
            ...
        }
    }
    server
    {
      ...
    }
    ...     #http全局块
}
```

<br/>

#### 3、文件详情

```nginx
#这是一个注释

user  nginx nginxs;  #配置用户和组
worker_processes  1; #允许生成的进程数，默认为1

#设置错误日志路径，级别。这个设置可以放入全局块，http块，server块
#级别以此为：debug|info|notice|warn|error|crit|alert|emerg
error_log  logs/error.log;
error_log  logs/error.log  notice;
error_log  logs/error.log  info;

pid  logs/nginx.pid; #指定nginx进程运行文件存放地址

events {
    accept_mutex on;  #设置网路连接序列化，防止惊群现象发生，默认为on
    multi_accept on;  #设置一个进程是否同时接受多个网络连接，默认为off
    use epoll;        #事件驱动模型，select|poll|kqueue|epoll|resig|/dev/poll|eventport
    worker_connections  1024;    #最大连接数，默认为512
}

http {
    include       mime.types; #文件扩展名与文件类型映射表
    default_type  application/octet-stream;  #默认文件类型，默认为text/plain
    
    #自定义main日志格式
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"'; 
    access_log  logs/access.log  main; #设置访问日志

    sendfile        on; #允许sendfile方式传输文件，默认为off，可以在http块，server块，location块
    sendfile_max_chunk 100k;  #每个进程每次调用传输数量不能大于设定的值，默认为0，即不设上限
    #tcp_nopush     on;
    keepalive_timeout  65;  #连接超时时间，默认为75s，可以在http，server，location块

    gzip  on; #使用gzip压缩响应
    
    #是否拦截4xx和5xx错误信息到客户端，默认值off
    #可以在http，server，location块，与error_page配合使用
    fastcgi_intercept_errors on; 
    
    #负载均衡
    upstream mysvr {   
        server 127.0.0.1:7878;
        server 192.168.10.121:3333 backup;  #热备
    }

    server {
        listen       80; #监听端口
        server_name  localhost; #监听地址

        charset utf-8; #指定字符集添加到“Content-Type”响应头字段

        access_log  logs/host.access.log  main;

        location / {
            root   /usr/share/nginx/html; #根目录
            index  index.html index.htm;  #设置默认页
            keepalive_requests 120; #单连接请求上限次数
        }
        
        location  ~*^.+$ {  #请求的url过滤，正则匹配，~为区分大小写，~*为不区分大小写
            proxy_pass  http://mysvr;  #请求转向mysvr 定义的服务器列表
            
            #仅允许IPv4网络访问 10.1.1.0/16并且192.168.1.0/24 不包括地址192.168.1.1
            #以及IPv6网络2001:0db8::/32
            #如果有很多规则， 最好使用 ngx_http_geo_module模块变量
            deny  192.168.1.1; #拒绝的ip
            allow 192.168.1.0/24; #允许的ip
            allow 10.1.1.0/16;
            allow 2001:0db8::/32;
            deny  all;
        }
        
        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        location ~ \.php$ {
            proxy_pass   http://127.0.0.1;
        }

        # redirect server error pages to the static page /50x.html
        # 前提：设置fastcgi_intercept_errors on
        error_page 404 https://www.baidu.com; #错误页
        error_page 500 /500.html;
        location = /500.html {
            root   /usr/share/nginx/html;
        }
        
        # @定义一个命名的 location，使用在内部定向时
        error_page 502 503 504 @errpage;
        location @errpage {
            access_log  logs/host.access.log  main;
            proxy_pass  http://127.0.0.1;
        }
    }
    
    #other server
    server {
        listen       8000;
        server_name  www.somename.com;

        location / {
            root   /usr/share/nginx/html;
            index  index.html index.htm;
        }
    }


    #HTTPS server
    server {
        listen       443 ssl;
        server_name  localhost;

        #配置ssl证书路径
        ssl_certificate      /etc/nginx/cer/www.somename.com.pem;
        ssl_certificate_key  /etc/nginx/cer/www.somename.com.key;

        ssl_session_cache    shared:SSL:1m;
        ssl_session_timeout  5m;

        ssl_ciphers  HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers  on;

        location / {
            root   /usr/share/nginx/html;
            index  index.html index.htm;
        }
    }
}
```

变量说明：

- `remote_addr` 与`http_x_forwarded_for` 用以记录客户端的ip地址
- `remote_user` ：用来记录客户端用户名称
- `time_local` ： 用来记录访问时间与时区
- `request` ： 用来记录请求的url与http协议
- `status` ： 用来记录请求状态，成功是200
- `body_bytes_s ent` ：记录发送给客户端文件主体内容大小
- `http_referer` ：用来记录从那个页面链接访问过来的
- `http_user_agent` ：记录客户端浏览器的相关信息

<br/>

#### 4、简化nginx.conf文件

通过`include` 命令，按`server`引入

```nginx
# nginx.conf

user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log;
pid /run/nginx.pid;

# Load dynamic modules. See /usr/share/nginx/README.dynamic.
# 加载动态模块
include /usr/share/nginx/modules/\*.conf; #/usr/share/nginx/modules/*.conf;

events {
    worker_connections 1024;
}

http {
    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;

    # 从/etc/nginx/conf.d目录加载模块化配置
    # See http://nginx.org/en/docs/ngx_core_module.html#include
    include /etc/nginx/conf.d/\*.conf; #/etc/nginx/conf.d/*.conf

    server {
        listen       80 default_server;
        listen       [::]:80 default_server;
        root         /usr/share/nginx/html;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/\*.conf; #/etc/nginx/default.d/*.conf

        error_page 404 /404.html;
            location = /40x.html {
        }
    }
}
```

```nginx
# /etc/nginx/conf.d/example.conf

#负载均衡
upstream mysvr {
    ...
}

#HTTPS server
server {
    ...
}
```