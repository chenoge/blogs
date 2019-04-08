---
title: 数组reduce
date: 2019-04-08 15:05:18
tags: [reduce]
---

```
arr.reduce(callback[, initialValue])
```

- `callback`执行数组中每个值的函数，包含四个参数：
  - `accumulator`累计器，累计回调的返回值
    - 它是上一次调用回调时返回的累积值，或`initialValue`
  - `currentValue`数组中正在处理的元素
  - `currentIndex`数组中正在处理的当前元素的索引
    -  如果提供了`initialValue`，则起始索引号为0，否则为1
  - `array`调用`reduce()`的数组
- `initialValue`作为第一次调用 `callback函数时`的第一个参数的值
  -  如果没有提供初始值，则将使用数组中的第一个元素
  - 在没有初始值的空数组上调用 reduce 将报错

<!--more-->

注：如果没有提供`initialValue`，reduce 会从索引1的地方开始执行 callback 方法，跳过第一个索引。如果提供`initialValue`，从索引0开始。

<br/>



```javascript
var initialValue = 0;
var sum = [{x: 1}, {x:2}, {x:3}].reduce(function (accumulator, currentValue) {
    return accumulator + currentValue.x;
},initialValue)

console.log(sum) // logs 6
```

```javascript
// 降维
var flattened = [[0, 1], [2, 3], [4, 5]].reduce(( acc, cur ) => acc.concat(cur),[]);
// flattened is [0, 1, 2, 3, 4, 5]
```

