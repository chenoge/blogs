---
title: contenteditable与user-modify
date: 2018-07-21 16:08:24
tags: [contenteditable,user-modify]
---

# 元素可编辑

``` css
-webkit-user-modify: read-only | read-write | read-write-plaintext-only
```

```css
contenteditable=""
contenteditable="events"
contenteditable="caret"
contenteditable="plaintext-only"
contenteditable="true"
contenteditable="false"
```

<!--more-->

- read-only：只读
-  read-write ：读写，可以**输入富文本**
- read-write-plaintext-only：读写，只能**输入纯文本** 

#### 注：contenteditable中默认可以输入(粘贴)富文本 

<br/>