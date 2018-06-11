---
title: JS-Promise-all-race
date: 2018-03-13 21:02:59
tags: [javaScript,promise]
---

# 一、Promise.all()

```javascript
const p = Promise.all([p1, p2, p3]);
```

p的状态由p1、p2、p3决定，分成两种情况 :

- **只有p1、p2、p3的状态都变成fulfilled，p的状态才会变成fulfilled**，此时p1、p2、p3的**返回值组成一个数组**，传递给p的回调函数。 

- **只要p1、p2、p3之中有一个被rejected，p的状态就变成rejected**，此时第一个被reject的实例的返回值，会传递给p的回调函数。

<br/>

# 三、Promise.race()

```javascript
const p = Promise.race([p1, p2, p3]);
```

只要p1、p2、p3之中有一个实例**率先改变状态，p的状态就跟着改变**。那个率先改变的 Promise 实例的返回值，就传递给p的回调函数 

```javascript
const p = Promise.race([
    fetch('/resource-that-may-take-a-while'),
    new Promise(function (resolve, reject) {
        setTimeout(() => reject(new Error('request timeout')), 5000)
    })
]);

p.then(console.log).catch(console.error);
```

如果 5 秒之内fetch方法无法返回结果，变量p的状态就会变为rejected，从而触发catch方法指定的回调函数。 