---
title: arg_PARAMETER和is_args
date: 2019-04-03 15:04:23
tags: [nginx,$is_args,$arg_PARAMETER]
---

#### `$arg_PARAMETER`

```
$arg_PARAMETER	#这个变量包含GET请求中，如果有变量PARAMETER时的值
$args	#这个变量等于请求行中(GET请求)的参数，如：foo=123&bar=blahbla
$is_args	#如果有args参数，这个变量等于"?"，否则等于""(空值)
```

<!--more-->

<br/>



#### `rewrite`重写URL参数

`/plaza/searchAll.html?tb_search=连衣裙&type=[item|shop]`

`/m/search/searchlist/index.html?keyword=连衣裙&type=[商品|店铺]`

```nginx
rewrite ^/plaza/searchAll.html /m/search/searchlist/index.html?keyword=$arg_tb_search&type=$arg_type? permanent;
# 使用arg_参数名（arg_type），去匹配到具体参数所带的值
# 最后的?可以阻止请求中原来的参数再带过来放到重写后的url里
```

注：

- `rewrite`只能对域名后边的**除去传递的参数外**的字符串起作用

- 默认的情况下，`Nginx`在进行`rewrite`后，都会自动添加上**旧地址中的参数部分**

<br/>

