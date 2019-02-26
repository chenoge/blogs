---
title: rel-noopener
date: 2019-02-26 11:52:09
tags: [rel,noopener]
---

#### `opener` 

`opener`属性是一个**可读可写**的属性，可返回对创建该窗口的 `Window `对象的引用。当你使用 `target='_blank'` 打开一个新的标签页时，新页面的 `window` 对象上有一个属性 `opener`，它指向的是前一个页面的 `window` 对象。但是，后一个页面对前一个页面的控制，受同源策略限制 。

<!--more-->

<br/>

#### `noopener` 

```html
<a href="http://example.com" target="_blank" rel="noopener">Example site</a>
```

如果使用了`rel="noopener"`属性，新页面的`window.opener`属性值为`null`。

<br/>

#### 同源策略

如果**后一个页面**与**前一个页面**`不同域`，`源安全模型`会阻止`window.opener`读取**前一个页面**的信息，但利用某些陈旧的API可以把你的页面导航到不同的URL：

```javascript
window.opener.location = newURL
```

<br/>