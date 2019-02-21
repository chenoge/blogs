---
title: nginx-serverName
date: 2019-02-21 15:38:56
tags: [nginx,server_name]
---

```nginx
# 确切的server_name匹配
server {
    listen       80;
    server_name  example.org  www.example.org;
}

# 以*通配符开始的最长字符串
server {
    listen       80;
    server_name  *.example.org;
}

# 以*通配符结束的最长字符串
server {
    listen       80;
    server_name  mail.*;
}

# 匹配正则表达式
server {
    listen       80;
    server_name  ~^(?<user>.+)\.example\.net$;
}
```

`nginx`将按照以下顺序对`server_name`进行匹配，**和配置文件中的出现顺序无关**

1. 确切的名字
2. 以星号开头的最长通配符名称
3. 最长的通配符名称以星号结尾
4. 第一个匹配正则表达式（**按配置文件中的出现顺序**）

**注：只要有一项匹配以后就会停止搜索** 

<!--more-->

<br/>

#### 通配符

- 通配符名称只可以在名称的**起始处或结尾处**包含一个星号，并且**星号**与其他字符之间**用点**分隔
- `www.*.example.org`和`w*.example.org`都是非法的，请使用`~^www\..+\.example\.org$` 和`~^w.*\.example\.org$`代替

<br/>

#### 正则表达式

- 为了使用正则表达式，虚拟主机名必须以波浪线`~`起始否则该名字会被认为是个**确切的名字**  
- 如果表达式含**星号**，则会被认为是个通配符名称
- 不要忘记设置“`^`”和“`$`”锚点，        语法上它们不是必须的，但是逻辑上是的
- 域名中的点`.`需要用反斜线`\`转义。含有`{`和`}`的正则表达式需要被引用

```nginx
server_name  "~^(?<name>\w\d{1,3}+)\.example\.net$";
```

<br/>

 命名的正则表达式捕获组在后面**可以作为变量使用**： 

```nginx
server {
    server_name   ~^(www\.)?(?<domain>.+)$;
    location / {
        root   /sites/$domain;
    }
}
```

`PCRE`使用下面语法支持命名捕获组：

> | `?<name>`  | 从PCRE-7.0开始支持，兼容Perl 5.10语法 |
> | ---------- | ------------------------------------- |
> | `?'name'`  | 从PCRE-7.0开始支持，兼容Perl 5.10语法 |
> | `?P<name>` | 从PCRE-4.0开始支持，兼容Python语法    |

<br/>

或者

```nginx
server {
    server_name   ~^(www\.)?(.+)$;
    location / {
        root   /sites/$2;
    }
}
```

<br/>

#### 杂项名称 

##### 空：`""` 

如果服务器块中未定义`server_name`，则`nginx`使用空名称作为服务器名称 

```nginx
server {
    listen       80;
    server_name  example.org  www.example.org  "";
}
```

##### 下划线：`_` 

```nginx
server {
    listen       80  default_server;
    server_name  _;
    return       444;
}
# 这个名称没什么特别之处，它只是无数域名之一，永远不会与任何真实姓名相交
# 使用"--"、"!@#"效果也是一样
```

##### default_server

```nginx
http {
    server {
        listen 80;
        server_name www.a.com;
    }
    
    # 显示的定义一个 default server
    server {
        listen 80 default_server;
        server_name _;
        return 403; # 403 forbidden
    }
    
}
```

```nginx
http {
    # 如果没有显式声明 default server 则第一个 server 会被隐式的设为 default server
    server {
        listen 80;
        server_name _; # _ 并不是重点
        return 403; # 403 forbidden
    }
    
    server {
        listen 80;
        server_name www.a.com;
    }
}
```

<br/>