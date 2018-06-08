---
title: drop-shadow与box-shadow
date: 2018-05-15 12:50:21
tags: [css,drop-shadow,box-shadow]
---

# 一、区别

- drop-shadow没有内阴影效果
- drop-shadow不能阴影叠加
- **drop-shadow穿透代码构建的元素的透明部分**
- **drop-shadow 也可以穿透PNG图片的透明部分**

![drop-shadow与box-shadow](CSS-drop-shadow与box-shadow\13123.png)

<font color=#A52A2A size=4 >**可以利用这个特性实现css 改变图片颜色**</font>

<br/>

# 二、利用box-shadow实现遮罩层

``` css
.spectiveBlur {
    position: absolute;
    top: 50%;
    left: 50%;
    width: 220px;
    line-height: 160px;
    transform: translate(-50%, -50%);
    border-radius: 10px;
    overflow: hidden;
    background: #E91E63;
    color: #fff;
    font-size: 200%;
    text-align: center;
    cursor: pointer;
    transition: transform .2s;
}

.spectiveBlur:hover {
    box-shadow: 0 0 0 1920px rgba(0, 0, 0, .7);
    transform: translate(-50%, -50%) scale(1.2);
}
```

