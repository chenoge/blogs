---
title: 根据规范重写Promise
date: 2018-06-01 15:07:40
tags: [javaScript,promise,规范]
---

# 零、[原文链接](https://mengera88.github.io/2017/05/18/Promise%E5%8E%9F%E7%90%86%E8%A7%A3%E6%9E%90/)

<br/>

# 一、使用场景

 

``` javascript
getUserId().then(getUserJobById).then(function(job) {
    // 对job的处理
});


function getUserJobById(id) {
    return new Promise(function(resolve) {
        http.get(baseUrl + id, function(job) {
            resolve(job);
        });
    });
}
```

<br/>

# 二、具体实现

``` javascript
function Promise(fn) {
    //一个promise对象有状态、值、回调callbacksList
    //then()可以别调用多次，所以是callbacksList
    var state = 'pending',
        value = null,
        callbacks = [];

    //then()函数必须返回一个promised对象
    //then()函数功能类似于订阅者模式中的listen()
    this.then = function(onFulfilled, onRejected) {
        return new Promise(function(resolve, reject) {
            handle({
                onFulfilled: onFulfilled || null,
                onRejected: onRejected || null,
                resolve: resolve,
                reject: reject
            });
        });
    };


    function handle(callback) {
        if (state === 'pending') {
            callbacks.push(callback);
            return;
        }

        var cb = state === 'fulfilled' ? callback.onFulfilled : callback.onRejected,
            ret;
        
        if (cb === null) {
            //如果 onFulfilled 不是函数且 promise1 成功执行， promise2 必须成功执行并返回相同的值
            //如果 onRejected 不是函数且 promise1 拒绝执行， promise2 必须拒绝执行并返回相同的据因
            cb = state === 'fulfilled' ? callback.resolve : callback.reject;
            cb(value);
            return;
        }
        
        //如果 onFulfilled 或者 onRejected 抛出一个异常 e ，则 promise2 必须拒绝执行，并返回拒因 e
        try {
            //cb()函数说明，只要父promise不抛出异常，子promise都成功执行
            //ret也可能是一个promise对象
            ret = cb(value);
            callback.resolve(ret);
        } catch (e) {
            callback.reject(e);
        }
    }
    
    //resolve()和reject()函数功能类似于订阅者模式中的trigger()
    function resolve(newValue) {
        //newValue可能是一个promise对象
        if (newValue && (typeof newValue === 'object' || typeof newValue === 'function')) {
            var then = newValue.then;
            if (typeof then === 'function') {
                //由newValue（promise）对象调用当前promise对象的resolve或reject
                then.call(newValue, resolve, reject);
                return;
            }
        }
        state = 'fulfilled';
        value = newValue;
        execute();
    }


    function reject(reason) {
        state = 'rejected';
        value = reason;
        execute();
    }


    function execute() {
        setTimeout(function() {
            callbacks.forEach(function(callback) {
                handle(callback);
            });
        }, 0);
    }


    fn(resolve, reject);
}
```

