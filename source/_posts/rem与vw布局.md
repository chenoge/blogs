---
title: rem与vw布局
date: 2017-11-26 20:57:34
tags: [rem,vw,自适应,自配]
---

# 一、rem布局

**rem布局**解决了一个问题，让设计稿在不同设备上进行**等比缩放**。 

```css
//媒体查询
@media screen and (max-width:359px) and (min-width:320px) {
    html,
    body {
        font-size: 50px !important;
    }
}

@media screen and (max-width:374px) and (min-width:360px) {
    html,
    body {
        font-size: 56.25px !important;
    }
}

@media screen and (max-width:413px) and (min-width:375px) {
    html,
    body {
        font-size: 58.5px !important;
    }
}

@media screen and (max-width:639px) and (min-width:414px) {
    html,
    body {
        font-size: 64.6px !important;
    }
}

@media screen and (min-width:640px) {
    html,
    body {
        font-size: 100px !important;
    }
}
```

```javascript
// onload事件、onresize事件监听
function refreshRem() {
    let docEl = window.document.documentElement;
    let width = docEl.getBoundingClientRect().width;
    if (width > 640) { width = 640 }
    let rem = 100 * (width / 640);
    docEl.style.fontSize = rem + "px";
}
```

**如果页面的宽度超过了640px，那么页面中`html`的`font-size`恒为`100px`，否则，页面中html的`font-size`的大小为： `100 \* (当前页面宽度 / 640)`**  

注：如果字体大小也等比缩放，在大屏手机上会有一种老人机的视觉

<br/>

# 二、vw布局

#### 方案一

最简单的方案就是所有的布局元素及属性都用VW来做单位，对应关系是： 

设计稿 750px——>100vw 

那我们书写时计算：`(x/750)*100vw` 

```scss
$vw_base: 750; 
@function vw($px) {
    @return ($px / 750) * 100vw;
}
```

<br/>

#### 方案二

沿用rem布局方案，所有的布局元素及属性都用rem做单位，用vw单位给html设置font-size形成“流单位”，这样就不再需要JS来动态计算根元素字体大小。

如果以前你习惯了约定750px设计稿的根元素字体大小为100px的话，你可以直接设置：

```css
html{
    font-size: 13.3333vw;
}
```