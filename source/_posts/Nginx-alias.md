---
title: Nginx-alias
date: 2017-09-27 20:00:09
tags: [Nginx,alias]
---

# `root&alias`文件路径配置

`root/alias`  是指定文件路径的两种方式，主要区别就是怎么解析`location`后面的`uri`

```markdown
http://localhost/appImg/abc.jpg
```

```nginx
location ^~ /appImg/ {
    root /home/nginx;
}
# 这个location相当于访问服务器上的文件路径：/home/nginx/appImg/abc.jpg 
```

```nginx
location ^~ /appImg/ {
    alias /home/nginx/;
}
# 这个location相当于访问服务器上的文件目录：/home/nginx/abc.jpg
```

