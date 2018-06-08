---
title: CSS-几个好用的属性
date: 2018-05-08 16:18:02
tags: [css]
---

# 1.attr

``` css
<p data-unit="元">剩余话费40</p>

[data-unit]:after {
    content: attr(data-unit);
    color: #3b98e0;
}
```



# 2. currentColor是color属性的值

``` css
.box {
    width: 200px;
    height: 200px;
    color: #3b98e0;
    border: 1px solid currentColor;
}
```



# 3.user-select 禁止选择文本

``` css
.box-1 {
    -webkit-user-select: none;
    -moz-user-select: none;
    user-select: none;
}
```



# 4.selection 可设置文字被选择时的样式

``` css
::selection {
    background: #FE6E66;
    color: #FFF;
}
```

**只能向 ::selection 选择器应用少量 CSS 属性：color、background、cursor 以及 outline**。