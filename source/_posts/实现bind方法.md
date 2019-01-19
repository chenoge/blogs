---
title: 实现bind方法
date: 2019-01-19 18:35:04
tags: [bind]
---
##### 手写bind方法

```javascript
Function.prototype.bind = function() {
    let fn = this;
    let args = Array.prototye.slice.call(arguments);
    let context = args.shift();
    return function() {
        return fn.apply(context, args.concat(Array.prototype.slice.call(arguments)));
    };
}
// 通过数据劫持(或者装饰模式)，就可以实现vue-methods方法中永远绑定当前实例
```

<!--more-->



