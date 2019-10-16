---
title: IE中的CSS兼容性问题
date: 2019-10-16 09:31:59
tags: [IE,CSS,兼容性]
---

```
1、IE9+不支持CSS变量、@supports

2、IE9+不支持object-fit

3、IE9-不支持flex布局

4、IE10+独有的属性-ms-high-contrast
@media (-ms-high-contrast: active),  (-ms-high-contrast: none)  {
    .class {
        color: red;
    }
}
```

