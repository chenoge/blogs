---
title: js-match
date: 2019-05-30 13:18:37
tags: [javascript,match]
---

#### match

检索返回一个字符串匹配正则表达式的的结果

```javascript
str.match(regexp)
```

- 参数
  
- regexp：如果没有任何参数并直接使用match() 方法 ，你将会得到一 个**包含空字符串的数组**
  
- 返回值
  - 如果使用g标志，则将返回与完整正则表达式匹配的所有结果，但不会返回捕获组，或者未匹配 `null`
  - 如果未使用g标志，则仅返回第一个完整匹配及其相关的捕获组。 在这种情况下，返回的项目将具有如下所述的其他属性，或者未匹配 `null`

- 附加属性

  如上所述，匹配的结果包含如下所述的附加特性
  - `groups`: 一个捕获组数组或 `undefined`（如果没有定义命名捕获组）
  - `index`: 匹配的结果的开始位置
  - `input`: 搜索的字符串

<!--more-->

<br/>



注：如果正则表达式不包含 `g `标志，`str.match()` 将返回与 `RegExp.exec()`相同的结果

```javascript
let str = 'For more information, see Chapter 3.4.5.1';
let re = /see (chapter \d+(\.\d)*)/i;
let found = str.match(re);

console.log(found);

// logs [ 'see Chapter 3.4.5.1',
//        'Chapter 3.4.5.1',
//        '.1',
//        index: 22,
//        input: 'For more information, see Chapter 3.4.5.1' ]

// 'see Chapter 3.4.5.1' 是整个匹配。
// 'Chapter 3.4.5.1' 被'(chapter \d+(\.\d)*)'捕获。
// '.1' 是被'(\.\d)'捕获的最后一个值。
// 'index' 属性(22) 是整个匹配从零开始的索引。
// 'input' 属性是被解析的原始字符串。
```



##### 命名捕获分组

```javascript
function toLocalDate(date){
  return date.replace (
  /(?<month>\d{2})-(?<day>\d{2})-(?<year>\d{4})/, 
  "$<day>-$<month>-$<year>"
  )
}
```
