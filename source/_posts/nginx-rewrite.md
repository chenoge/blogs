---
title: nginx-rewrite
date: 2018-04-10 17:41:04
tags: [nginx,rewrite]
---

# 一、rewrite语法

```nginx
    rewrite    <regex>    <replacement>     [flag]       
    # 关键字     正则       替代内容         flag标记
```

- flag标记说明：
  - **last**  #本条规则匹配完成后，**继续向下匹配新的location URI规则**
  - **break**  #本条规则匹配完成即终止，**不再匹配后面的任何规则**
  - **redirect**  #返回**302临时重定向**，浏览器地址会**显示跳转后的URL地址**
  - **permanent**  #返回**301永久重定向**，浏览器地址栏会**显示跳转后的URL地址**

```nginx 
rewrite ^/(.*) http://www.czlun.com/$1 permanent;
# regex部分是 ^/(.*) ，这是一个正则表达式，匹配完整的域名和后面的路径地址
# replacement部分是http://www.czlun.com/$1，$1是取自regex部分()里的内容。匹配成功后跳转到的URL。
# flag部分 permanent表示永久301重定向标记，即跳转到新的 http://www.czlun.com/$1 地址上
```

<br/>

## 二、正则表达式

|   字符    | 描述                                                         |
| :-------: | :----------------------------------------------------------- |
|     \     | 转义字符。如“\n”匹配一个换行符                               |
|     ^     | 匹配输入字符串的**起始位置**                                 |
|     $     | 匹配输入字符串的**结束位置**                                 |
|     *     | 匹配前面的字符**零次或多次**。如“ol*”能匹配“o”及“ol”、“oll”  |
|     +     | 匹配前面的字符**一次或多次**。如“ol+”能匹配“ol”及“oll”、“oll”，但不能匹配“o” |
|     ?     | 匹配前面的字符**零次或一次**，例如“do(es)?”能匹配“do”或者“does”，"?"等效于"{0,1}" |
|     .     | 匹配除“\n”之外的任何单个字符。<br />若要匹配包括“\n”在内的任意字符，请使用诸如“[.\n]”之类的模式。 |
| (pattern) | **匹配括号内pattern**并可以**在后面获取对应的匹配。<br />**常用$0...$9属性获取小括号中的匹配内容，要匹配圆括号字符需要\(Content\) |

```nginx
server {
    listen 80;
    server_name abc.com www.abc.com;
    if ( $host != 'www.abc.com'  ) {
        rewrite ^/(.*) http://www.abc.com/$1 permanent;
    }
    location / {
        root /data/www/www;
        index index.html index.htm;
    }
    error_log  logs/error_www.abc.com.log  error;
    access_log  logs/access_www.abc.com.log  main;
}
```

<br/>

# 三、逻辑判断

| 操作符  | 含义                                                         |
| ------- | ------------------------------------------------------------ |
| =，!=   | 比较的一个变量和字符串。                                     |
| ~，~*   | 与正则表达式匹配的变量，如果这个正则表达式中包含 **}，;** 则整个表达式需要用**"或'**包围。 |
| -f，!-f | 检查一个**文件是否存**在。                                   |
| -d，!-d | 检查一个**目录是否存在**。                                   |
| -e，!-e | 检查一个文件、目录、符号链接是否存在。                       |
| -x，!-x | 检查一个文件**是否可执行**。                                 |

<br/>

# 四、常用变量

| 变量                | 含义                                                         |
| :------------------ | ------------------------------------------------------------ |
| **$args**           | 这个变量等于请求行中的参数，同$query_string                  |
| $content_length     | 请求头中的Content-length字段。                               |
| **$content_type**   | 请求头中的Content-Type字段。                                 |
| $document_root      | 当前请求在root指令中指定的值。                               |
| **$host**           | 请求主机头字段，否则为服务器名称。                           |
| $http_user_agent    | 客户端agent信息                                              |
| **$http_cookie**    | 客户端cookie信息                                             |
| $limit_rate         | 这个变量可以限制连接速率。                                   |
| **$request_method** | 客户端请求的动作，通常为GET或POST。                          |
| **$remote_addr**    | 客户端的IP地址。                                             |
| **$remote_port**    | 客户端的端口。                                               |
| $remote_user        | 已经经过Auth   Basic Module验证的用户名。                    |
| $request_filename   | 当前请求的文件路径，由root或alias指令与URI请求生成。         |
| **$scheme**         | HTTP方法（如http，https）。                                  |
| $server_protocol    | 请求使用的协议，通常是HTTP/1.0或HTTP/1.1。                   |
| $server_addr        | 服务器地址，在完成一次系统调用后可以确定这个值。             |
| **$server_name**    | 服务器名称。                                                 |
| **$server_port**    | 请求到达服务器的端口号。                                     |
| **$request_uri**    | **包含请求参数的原始URI**，不包含主机名，如”/foo/bar.php?arg=baz”。 |
| **$uri**            | **不带请求参数的当前URI**，$uri不包含主机名，如”/foo/bar.html”。 |
| $document_uri       | 与$uri相同。                                                 |

```nginx
# 限制浏览器访问  
if ($http_user_agent ~ Firefox) {   
    rewrite ^(.*)$ /firefox/$1 break;   
}

if ($http_user_agent ~ MSIE) {   
    rewrite ^(.*)$ /msie/$1 break;   
}      

if ($http_user_agent ~ Chrome) {   
    rewrite ^(.*)$ /chrome/$1 break;   
}  

# 限制IP访问  
if  ($remote_addr = 192.168.197.142) {  
    return 403;  
}

# set指令是设置变量用的,可以用来达到多条件判断时作标志用
# 判断IE并重写,且不用break;我们用set变量来达到目的
if ($http_user_agent ~* msie) {  
    set $isie 1;  
}

if ($fastcgi_script_name = ie.html) {  
    set $isie 0;  
}  
		  
if ($isie 1) {  
    rewrite ^.*$ ie.html;  
}  
```

