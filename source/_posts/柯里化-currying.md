---
title: 柯里化-currying
date: 2018-10-11 20:20:12
tags: [柯里化,currying]
---

# 什么是柯里化？

在计算机科学中，柯里化**（Currying）**是把接受**多个参数**的函数变换成接受一个**单一参数**（最初函数的第一个参数）的函数，并且返回接受**余下的参数**且返回结果的新函数的技术。 

```javascript
function currying(fn) {
    var slice = Array.prototype.slice,
        __args = slice.call(arguments, 1);
    return function() {
        var __inargs = slice.call(arguments);
        return fn.apply(null, __args.concat(__inargs));
    };
}
```

注：柯里化主要是利用闭包，将**单一参数**封装起来。

```javascript
function square(i) {
    return i * i;
}

function map(handeler, list) {
    return list.map(handeler);
}

// 通用性写法
map(square, [1, 2, 3, 4, 5]);
map(square, [6, 7, 8, 9, 10]);
map(square, [10, 20, 30, 40, 50]);

// 柯里化写法
// 固定mapSQ的单一参数，降低通用性，提高适用性
var mapSQ = currying(map, square);  // square 为单一参数
mapSQ([1, 2, 3, 4, 5]);
mapSQ([6, 7, 8, 9, 10]);
mapSQ([10, 20, 30, 40, 50]);

// 通用设计与专门设计相比，功能更多，但效率却更低。
```

<!--more-->

<br/>

# 柯里化的用途

1. **降低通用性，提高适用性**

2. **延迟执行**

   ```javascript
   var add = function() {
       var _this = this,
           _args = arguments;
       return function() {
           if (!arguments.length) {
               var sum = 0;
               for (var i = 0, c; c = _args[i++];) {
                   sum += c;
               }
               return sum;
           } else {
               Array.prototype.push.apply(_args, arguments);
               return arguments.callee;
           }
       }
   }
   add(1)(2)(3)(4)(); //10
   ```

   