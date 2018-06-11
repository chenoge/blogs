---
title: nginx-try_files
date: 2017-09-30 16:04:03
tags: [nginx,try_files]
---

# 一、try_files指令

- 语法：**try_files file ... uri**  或  **try_files file ... = code**
- 默认值：无
- 作用域：**server location** 

**按顺序检查文件是否存在，<font color=#A52A2A size=4 >返回第一个找到的文件或文件夹</font>(结尾加斜线表示为文件夹)，如果所有的文件或文件夹都找不到，会<font color=#A52A2A size=4 >进行一个内部重定向到最后一个参数</font>。**

<br/>

# 二、例子说明

```nginx
try_files /app/cache/   $uri   @fallback; 
```

它将检测**/app/cache/index.php**、**/app/cache/index.html**、**$uri**是否存在，如果不存在着内部重定向到**@fallback**(＠表示配置文件中预定义标记点) 。你也可以使用**一个文件或者状态码**(**=404**)作为最后一个参数，如果是最后一个参数是文件，那么这个文件必须存在。