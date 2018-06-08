---
title: 整页滚动
date: 2018-05-27 22:48:11
tags: [javaScript,滚动]
---


``` javascript
let delay = 800;
let pageHeight = 500;
let preNow = new Date();
let el = document.getElementById('demo');
let scrollTop = el.scrollTop;
el.addEventListener('scroll', myFunction);

function myFunction(e) {
    //节流计时器
    let now = new Date();
    if (now - preNow > delay) {
        preNow = now;
        if (el.scrollTop > scrollTop) {
            el.scrollTop += pageHeight;
        } else {
            if (el.scrollTop < scrollTop) {
                el.scrollTop -= pageHeight;
            }
        }
        scrollTop = el.scrollTop;
    } else {
        el.scrollTop = scrollTop;
    }
}

```

