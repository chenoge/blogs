---
title: CSS-滚动条跳动问题
date: 2018-05-24 11:33:27
tags: [css,滚动条]
---

# 一、问题描述

 信息流页面是从上往下push渲染的。开始只有头部一些信息加载，此时页面高度有限，没有滚动条；然后，更多内容显示，滚动条出现，<font color=#A52A2A size=4 >**占据可用宽度**</font>，margin: 0 auto**主体元素**自然会做**偏移**——<font color=#A52A2A size=4 >**跳动产生**</font>。<br/>

# 二、利用calc和vw

``` css
.wrap-outer {
    margin-left: calc(100vw - 100%);
}

/*或者：*/
.wrap-outer {
    padding-left: calc(100vw - 100%);
}
```

- 100vw相对于浏览器的window.innerWidth，是浏览器的内部宽度，注意，滚动条宽度也计算在内！

- 100%是可用宽度，是不含滚动条的宽度。<br/>

# 三、只利用vw

``` css
.wrap-outer {
    width: 100vw;
    overflow-x: hidden;
}
```