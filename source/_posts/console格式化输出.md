---
title: console.log格式化输出
date: 2018-12-07 16:43:54
tags: [console.log]
---

#### 字符串替代和格式设置

传递到任何记录方法的**第一个参数可能包含一个或多个格式说明符**。格式说明符由一个 `%` 符号与后面紧跟的一个字母组成，字母指示应用到值的格式。**字符串后面的参数会按顺序应用到占位符**。

```javascript
 // Sam has 100 points
console.log("%s has %d points", "Sam", 100);

// Node count: 2, and the time is 1544184144238.
console.log("Node count: %d, and the time is %f.", document.childNodes.length, Date.now());
```


格式说明符的完整列表为： 

| 说明符   | 输出                                                         |
| -------- | ------------------------------------------------------------ |
| %s       | 将值格式化为字符串                                           |
| %i 或 %d | 将值格式化为整型                                             |
| %f       | 将值格式化为浮点值                                           |
| %o       | 将值格式化为可扩展 DOM 元素。如同在 Elements 面板中显示的一样 |
| %O       | 将值格式化为可扩展 JavaScript 对象                           |
| %c       | 将 CSS 样式规则应用到第二个参数指定的输出字符串              |