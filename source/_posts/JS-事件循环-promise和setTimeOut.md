---
title: JS-事件循环-promise和setTimeOut
date: 2017-11-09 14:13:04
tags: [javaScript,事件循环,event-loop,promise,setTimeOut]
---

# 一、事件循环

1. `mocrotasks` 队列的第一个任务取出,放到`执行栈`中，开始执行
2. 执行直到当前      stack 为空，于是去检查 `microtasks `队列
3. 依次执行` microtasks` 队列的所有任务，直到 `microtasks` 队列为空，转 1 

<br/>

注：

- 如上循环中，注意到第      2 步中执行时可以向 `microtasks` 队列压入 `microtask`
- 一个事件循环里，`任务队列`分为`mocrotasks`队列和`microtasks`队列
- `microtasks`队列只有一个，`mocrotasks`**队列可以有多个**

<br/>

# 二、Macrotask

- setImmediate
- setTimeout
- setInterval

<br/>

# 三、Microtask

- process.nextTick
- Promise
- Object.observe
- MutaionObserver

<br/>

# 四、例子

```javascript
// demo01
(function test() {
    // 1 task A 执行中
    // 2 tasks 队列压入新的 task B
    setTimeout(() => {
        // 9 microtasks 队列为空，于是检查 tasks 队列，取出 B并执行了
        console.log(4)
    }, 0)
    new Promise(resolve => {
            console.log(1)
            for (var i = 0; i < 10000; i++) {
                i == 9999 && resolve()
            }
            // 3 task A 继续执行
            console.log(2)
        })
        // 4 microtasks 队列压入 microtask a
        .then(() => {
            // 6 microtask a 执行中
            console.log(5)
            Promise.resolve(7)
                // 7 microtasks 队列压入 microtask b
                .then(v => console.log(v))
            // microtask a 执行完毕
        })
        // 8 microtasks 队列压入 microtask c
        // 这个 then 执行完后继续检查 microtasks 队列，并一次执行 b，c
        .then(() => {
            console.log(6)
        })
    // 5 task A 执行完毕，检查 microtasks 队列，发现非空，执行 microtasks 队列的第一个 microtask a
    console.log(3)
})();

/**
1
2
3
5
7
6
4
*/
```

<br/>

<!--more--> 

[原文](https://www.cnblogs.com/jymz/p/7900439.html)

```javascript
// demo02
// 为了方便理解，我以打印出来的字符作为当前的任务名称
// setTimeout/Promise等我们称之为任务源。而进入任务队列的是他们指定的具体执行任务。
setTimeout(function() {
    console.log('timeout1');
})
 
new Promise(function(resolve) {
    console.log('promise1');
    for(var i = 0; i < 1000; i++) {
        i == 99 && resolve();
    }
    console.log('promise2');
}).then(function() {
    console.log('then1');
})
 
console.log('global1');
```

首先，事件循环从宏任务队列开始，这个时候，宏任务队列中，只有一个script(整体代码)任务。每一个任务的执行顺序，都依靠**函数调用栈**来搞定，而当遇到**任务源**时，则会先分发任务到对应的队列中去。

![img](JS-事件循环-promise和setTimeOut\369c6fdc4dcbb43a78a1ac48365e25f8-2.png)

首先script任务开始执行，**全局上下文**入栈

<br/>

第二步：script任务执行时首先遇到了setTimeout，**setTimeout为一个宏任务源**，那么他的作用就是将任务分发到它对应的队列中。

![img](JS-事件循环-promise和setTimeOut\369c6fdc4dcbb43a78a1ac48365e25f8-3.png)

**宏任务timeout1进入setTimeout队列**

<br/>

第三步：script执行时遇到Promise实例。Promise构造函数中的第一个参数，是在new的时候执行，因此不会进入任何其他的队列，而是直接在当前任务直接执行了，而**后续的.then则会被分发到micro-task的Promise队列中去**。因此，构造函数执行时，里面的参数进入函数调用栈执行。for循环不会进入任何队列，因此代码会依次执行。

![img](JS-事件循环-promise和setTimeOut\8c1e0120dbe950b64c63dfd9010c82e6-4.jpg)

promise1入栈执行，这时promise1被最先输出

![img](JS-事件循环-promise和setTimeOut\8c1e0120dbe950b64c63dfd9010c82e6-5.png)

resolve在for循环中入栈执行

![img](JS-事件循环-promise和setTimeOut\26f371cf80c7c4147ebb84af03402437-6.png)

构造函数执行完毕的过程中，resolve执行完毕出栈，promise2输出，promise1页出栈，then执行时，Promise任务then1进入对应队列

script任务继续往下执行，最后只有一句输出了globa1，然后，全局任务就执行完毕了。

<br/>

第四步：第一个宏任务script执行完毕之后，就开始执行所有的可执行的微任务。这个时候，微任务中，只有Promise队列中的一个任务then1，因此直接执行就行了，执行结果输出then1，当然，他的执行，也是进入函数调用栈中执行的。

![img](JS-事件循环-promise和setTimeOut\26f371cf80c7c4147ebb84af03402437-7.png)

执行所有的微任务

<br/>

第五步：当所有的micro-tast执行完毕之后，表示第一轮的循环就结束了。这个时候就得开始第二轮的循环。第二轮循环仍然从宏任务macro-task开始。

![img](JS-事件循环-promise和setTimeOut\a3a6b7679adfaa22423eb02abf45d574-8.jpg)

微任务被清空

这个时候，我们发现宏任务中，只有在setTimeout队列中还要一个timeout1的任务等待执行。因此就直接执行即可。

![img](JS-事件循环-promise和setTimeOut\a3a6b7679adfaa22423eb02abf45d574-9.png)

timeout1入栈执行

这个时候宏任务队列与微任务队列中都没有任务了，所以代码就不会再输出其他东西了。

<br/>

```javascript
// demo03
console.log('golb1');
 
setTimeout(function() {
    console.log('timeout1');
    process.nextTick(function() {
        console.log('timeout1_nextTick');
    })
    new Promise(function(resolve) {
        console.log('timeout1_promise');
        resolve();
    }).then(function() {
        console.log('timeout1_then')
    })
})
 
setImmediate(function() {
    console.log('immediate1');
    process.nextTick(function() {
        console.log('immediate1_nextTick');
    })
    new Promise(function(resolve) {
        console.log('immediate1_promise');
        resolve();
    }).then(function() {
        console.log('immediate1_then')
    })
})

 
process.nextTick(function() {
    console.log('glob1_nextTick');
})
new Promise(function(resolve) {
    console.log('glob1_promise');
    resolve();
}).then(function() {
    console.log('glob1_then')
})

 
setTimeout(function() {
    console.log('timeout2');
    process.nextTick(function() {
        console.log('timeout2_nextTick');
    })
    new Promise(function(resolve) {
        console.log('timeout2_promise');
        resolve();
    }).then(function() {
        console.log('timeout2_then')
    })
})

 
process.nextTick(function() {
    console.log('glob2_nextTick');
})
new Promise(function(resolve) {
    console.log('glob2_promise');
    resolve();
}).then(function() {
    console.log('glob2_then')
})

 
setImmediate(function() {
    console.log('immediate2');
    process.nextTick(function() {
        console.log('immediate2_nextTick');
    })
    new Promise(function(resolve) {
        console.log('immediate2_promise');
        resolve();
    }).then(function() {
        console.log('immediate2_then')
    })
})
```

第一步：宏任务script首先执行。全局入栈。glob1输出。

![img](JS-事件循环-promise和setTimeOut\5436da20c69cf399a2844e1fe9d4c92d-10.png)

script首先执行

<br/>

第二步，执行过程遇到setTimeout。setTimeout作为任务分发器，将任务分发到对应的宏任务队列中。

![img](JS-事件循环-promise和setTimeOut\5436da20c69cf399a2844e1fe9d4c92d-11.png)

timeout1进入对应队列

<br/>

第三步：执行过程遇到setImmediate。setImmediate也是一个宏任务分发器，将任务分发到对应的任务队列中。setImmediate的任务队列会在setTimeout队列的后面执行。

![img](JS-事件循环-promise和setTimeOut\300bd342a371cb5516c22a7a007975c1-12.png)

进入setImmediate队列

<br/>

第四步：执行遇到nextTick，process.nextTick是一个微任务分发器，它会将任务分发到对应的微任务队列中去。

![img](JS-事件循环-promise和setTimeOut\300bd342a371cb5516c22a7a007975c1-13.png)

nextTick

<br/>

第五步：执行遇到Promise。Promise的then方法会将任务分发到对应的微任务队列中，但是它构造函数中的方法会直接执行。因此，glob1_promise会第二个输出。

![img](JS-事件循环-promise和setTimeOut\7bcec423f310abbdd6137076868835ce-14.png)

先是函数调用栈的变化

![img](JS-事件循环-promise和setTimeOut\7bcec423f310abbdd6137076868835ce-15.png)

然后glob1_then任务进入队列

<br/>

第六步：执行遇到第二个setTimeout。

![img](JS-事件循环-promise和setTimeOut\3f5034d633271b5bbef1ebc0fd4b7128-16.png)

timeout2进入对应队列

<br/>

第七步：先后遇到nextTick与Promise

![img](JS-事件循环-promise和setTimeOut\3f5034d633271b5bbef1ebc0fd4b7128-17.png)

glob2_nextTick与Promise任务分别进入各自的队列

<br/>

第八步：再次遇到setImmediate。

![img](JS-事件循环-promise和setTimeOut\d3274c7906c631597cdc60df5badf864-18.png)

nextTick

这个时候，script中的代码就执行完毕了，执行过程中，遇到不同的任务分发器，就将任务分发到各自对应的队列中去。接下来，将会执行所有的微任务队列中的任务。

其中，nextTick队列会比Promie先执行。nextTick中的可执行任务执行完毕之后，才会开始执行Promise队列中的任务。

当所有可执行的微任务执行完毕之后，这一轮循环就表示结束了。下一轮循环继续从宏任务队列开始执行。

这个时候，script已经执行完毕，所以就从setTimeout队列开始执行。

![img](JS-事件循环-promise和setTimeOut\1c6a5d5efd3f4c3d7383719e70517bbc-19.png)





