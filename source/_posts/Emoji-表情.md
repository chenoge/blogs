---
title: Emoji-表情
date: 2018-02-28 21:52:49
tags: [Emoji]
---

# 一、码点和含义

Unicode 只是规定了 Emoji 的码点和含义，并没有规定它的样式，由各个系统自己实现 

<br/>

# 二、使用方式 

Emoji 虽然是文字，但是无法书写，必须使用其他方法插入文档。 

（1）最简单的方法当然是复制/粘贴，你可以到 [getEmoji.com](http://getemoji.com/) 选中一个 Emoji 贴在自己的文档即可。 



（2）另一种方法是通过码点输入 Emoji。以 HTML 网页为例，将码点U+1F600写成 HTML 实体的形式&#128512;（十进制）或&#x1F600;（十六进制），就可以插入网页。码点到这个[页面](http://emojipedia.org/facebook/http:/emojipedia.org/facebook/)查询。 



（3）JavaScript 输入 Emoji，可以使用 [node-emoji](https://www.npmjs.com/package/node-emoji) 这个库。 

```javascript
var emoji = require('node-emoji');

// 返回 coffee 的 Emoji
emoji.get('coffee');

// 返回文字标签对应的 Emoji
// https://www.webpagefx.com/tools/emoji-cheat-sheet/
emoji.get(':fast_forward:');

// 将文字替换成 Emoji
emoji.emojify('I :heart: :coffee:!');

// 随机返回一个 Emoji 
emoji.random();

// 查询 Emoji
// 返回结果是一个数组 
emoji.search('cof');
```





（4）还可以通过 [CSS](https://afeld.github.io/emoji-css/) 插入 Emoji。 

```html
<link href="https://afeld.github.io/emoji-css/emoji.css" rel="stylesheet">
<i class="em em-baby"></i>
```

