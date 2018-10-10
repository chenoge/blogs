---
title: promise、async和await的执行顺序
date: 2018-10-10 11:35:22
tags: [promise,async,await]
---

## 1、题目和答案

一道题题目：下面这段promise、async和await代码，请问控制台打印的顺序？ 

```javascript
async function async1() {
    console.log('async1 start')
    await async2()
    console.log('async1 end')
}

async function async2() {
    console.log('async2')
}

console.log('script start')

setTimeout(function() {
    console.log('setTimeout')
}, 0)

async1();

new Promise(function(resolve) {
    console.log('promise1')
    resolve();
}).then(function() {
    console.log('promise2')
})

console.log('script end')
```

<!--more-->

上述，在`Chrome 66`和`node v10`中，正确输出是： 

```markdown
script start
async1 start
async2
promise1
script end
promise2
async1 end
setTimeout
```

<br/>

## 2、知识点

显然，这考察的是js中的事件循环和回调队列。注意以下几点：

- `Promise`优先于`setTimeout`宏任务。所以，`setTimeout`回调会在最后执行。
- `Promise`一旦被定义，就会立即执行。
- `Promise`的`reject`和`resolve`是异步执行的回调。所以，`resolve()`会被放到回调队列中，在主函数执行完和`setTimeout`前调用。
- `await`执行完后，会让出线程。`async`标记的函数会返回一个`Promise`对象

<br/>

## 3、 难点

> 最令人困惑的，就是`async1 end`在`promise2`之后输出

<br/>

## 4、 猜测await表达式

```javascript
[return_value] = await expression; 
```

```javascript
// expression 可能被转换成如下代码
function _expression() {
    let result = eval(expression);
    let isPromise = result.toString().includes('Promise');
    if (isPromise) {
        result.then(Promise.resolve, Promise.reject);
    } else {
        return Promise.resolve(result);
    }
}
// 无果
// 这段时间太忙了（2018/10/10），逃~~~
```

