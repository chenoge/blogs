---
title: nginx使用include命令拆分配置文件
date: 2019-01-03 21:36:53
tags: [nginx,include]
---
#### [include](http://nginx.org/en/docs/ngx_core_module.html#include) 

```
Syntax:	include file | mask;
Default: —
Context: any
```

Includes another `file`, or files matching the specified `mask`, into configuration. Included files should consist of syntactically correct directives and blocks. 

```nginx
include mime.types;
include vhosts/*.conf; #通配符写法
```

注：适用范围是任何地方，`include`命令相当于是复制文件内容到引用的地方

<br/>