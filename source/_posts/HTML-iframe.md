---
title: HTML-iframe
date: 2018-05-21 11:53:42
tags: [html,iframe]
---

# 一、通过window获取iframe

window.frames是个伪数组，可以通过window.frames[index]或window.frames[name]来获取iframe 

<br/>

# 二、通过iframe获取window、document

如果想获取iframe里的window或者document，可以使用 

- iframe.contentWindow
- iframe.contentDocument 

<font color=#A52A2A size=4 >**注：跨域iframe没有操作权限**</font>

<br/>

# 三、window获取顶级窗口、父窗口

- 获取顶级窗口：window.top 
- 获取父级窗口：window.parent 