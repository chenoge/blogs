---
title: JS-代码异常监控
date: 2018-05-16 12:43:53
tags: [javaScript,异常监控]
---

# 一、window.onerror

``` javascript
window.onerror = function(message, source, lineno, colno, error) { ... }
```

函数参数：

- message：错误信息（字符串）。可用于HTML onerror=""处理程序中的event。
- source：发生错误的脚本URL（字符串）
- lineno：发生错误的行号（数字）
- colno：发生错误的列号（数字）
- error：[Error对象](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Error)（对象）

**若该函数返回true，则阻止执行默认事件处理函数。** 

<br/>

# 二、window.addEventListener('error')

``` javascript
window.addEventListener('error', function(event) { ... })
```

**[ErrorEvent](https://developer.mozilla.org/zh-CN/docs/Web/API/ErrorEvent) 类型的event包含有关事件和错误的所有信息。<br/>

``` javascript
window.onerror = function(msg, url, lineNo, columnNo, error) {
    var message = [
        'Message: ' + msg,
        'URL: ' + url,
        'Line: ' + lineNo,
        'Column: ' + columnNo,
        'Error object: ' + JSON.stringify(error)
    ].join(' - ');

    console.log(message);
};
```



![前端代码异常监控曲线示例图](JS-代码异常监控\前端代码异常监控曲线示例图2.png)

