---
title: JS-函数单一职责原则
date: 2018-04-29 11:57:08
tags: [javaScript,单一职责]
---

​			函数违反单一原则最大一个后果就是会导致逻辑混乱。如果一个函数承担了太多的职责。

不妨试下：<font color=#A52A2A size=4 >**函数单一原则 -- 一个函数只做一件事</font>** 。

<br/>

##### 埋头就是干：

``` javascript
//图片预加载函数
let delayload = (function() {
    let img = document.querySelector("#img");
    img.src = "loading.gif";
    let newImg = document.createElement("img");
    newImg.onload = function() {
        img.src = newImg.src;
    }
    return function(src) {
        newImg.src = src;
    }
})();

delayload("jimmy.jpg");
```

<br/>

##### 遵循一个函数只做一件事后：

``` javascript
//将背景图设置，和图片加载的src修改分开
let delayload = (function() {
    let img = document.querySelector("#img");
     img.src = "loading.gif";
    return {
        setSrc: function(src) {
            img.src = src;
        }
    }
})();


let proxy = (function() {
    let img = document.createElement('img');
    img.onload = function() {
        delayload.setSrc(img.src);
    }
    return {
        setSrc: function(src) {
            img.src = src;
        }
    }
})();

proxy.setSrc("jimmy.jpg");
```