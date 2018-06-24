---
title: html-雪碧图或精灵图
date: 2017-11-27 18:21:30
tags: [雪碧图,精灵图]
---

​       网站为了减少http请求数，会将大量的图片图片合成一张**雪碧图**（Sprite）来使用。雪碧图的使用就是通过控制`background-position`属性值来确定图片呈现的位置。 

<!--more-->

![](html-雪碧图或精灵图\20160404214920203.png)

```html
<style>
    .icon1{background-position: 0 0;}
    .icon2{background-position: -40px 0;}
    .icon3{background-position: 0 -25px;}
    .icon4{background-position: -40px -25px;}
</style>
```

