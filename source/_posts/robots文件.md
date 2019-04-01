---
title: robots文件
date: 2019-04-01 14:32:24
tags: [robots,爬虫]
---

####  robots文件

`Robots`是站点与`spider`沟通的重要渠道，站点通过`robots`文件声明本网站中不想被搜索引擎收录的部分或者指定搜索引擎只收录特定的部分。仅当您的网站包含不希望被视频搜索引擎收录的内容时，才需要使用`robots.txt`文件。如果您希望搜索引擎收录网站上所有内容，请勿建立`robots.txt`文件。

`robots.txt`文件往往放置于根目录下，包含一条或更多的记录，这些记录通过空行分开（以`CR,CR/NL,or NL`作为结束符），每一条记录的格式如下所示：

```
<field>:<optional space><value><optionalspace>
```

<!--more-->

<br/>



#### 字段

`User-agent`：该项的值用于描述搜索引擎robot的名字。

- 在`robots.txt`文件中，如果有多条`User-agent`记录，说明有多个robot会受到`robots.txt`的限制
- 至少要有一条`User-agent`记录
- 如果该项的值设为`*`，则对任何`robot`均有效，这样的记录只能有一条



`Disallow`：该项的值用于描述不希望被访问的一组URL

- 可以是一条**完整路径**，也可以是路径的**非空前缀**
- 至少要有一条Disallow记录



`Allow`：该项的值用于描述希望被访问的一组URL，

- 与Disallow项相似，这个值可以是一条完整的路径，也可以是路径的前缀



<br/>

#### 使用`*`和`#### 

`Baiduspider`支持使用通配符`*`和`$`来模糊匹配url

- `*`匹配零或多个任意字符
- `$`匹配行结束符
- `# `表示注释

<br/>



```
# robots.txt generated at http://tool.chinaz.com/robots/ 

User-agent: Baiduspider
Disallow: /

User-agent: YodaoBot
Disallow: /

User-agent: *
Disallow: 
Disallow: /bin/
Sitemap: http://domain.com/sitemap.xml
```







