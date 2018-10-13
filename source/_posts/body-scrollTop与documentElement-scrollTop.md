---
title: document.body.scrollTop与document.documentElement.scrollTop
date: 2018-10-13 13:34:10
tags: [document.body.scrollTop,document.documentElement.scrollTop]
---

### 不同浏览器的 scrollTop差异:

- **IE6/7/8：**使用 `document.documentElement.scrollTop`； 
- **IE9及以上：**使用`window.pageYOffset`或者`document.documentElement.scrollTop `
- **Safari:**  `window.pageYOffset` 与document.body.scrollTop都可以； 
- **Firefox:** window.pageYOffset 或者 `document.documentElement.scrollTop `；
- **Chrome：**只认识`document.body.scrollTop`;

注：document.body.scrollTop与document.documentElement.scrollTop同时只会有一个值生效。

```javascript
let sTop = document.body.scrollTop || document.documentElement.scrollTop;
```



