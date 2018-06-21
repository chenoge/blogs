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

注：异步处理上，没有达到要求。具体参见[事件循环](/2017/11/09/JS-事件循环-promise和setTimeOut/)

<br/>

# 三、封装的代码

```javascript
// @/tool/axios.js
import qs from 'qs';
import iView from 'iview';
import axios from 'axios';

export default function (config) {
  if (config.isFormData) {
    config.data = qs.stringify(config.data);
    !config.headers && (config.headers = {});
    config.headers['Content-Type'] = 'application/x-www-form-urlencoded';
  }

  return axios(config).then(
    res => {
      if (res.data.status >= 1) {
        config.showSuccess && iView.Notice.success({
          duration: 2,
          title: '通知',
          desc: res.data.msg
        });
        return Promise.resolve(res);
      }

      if (res.data.status === 0) {
        !config.hiddenError && iView.Notice.error({
          duration: 2,
          title: '提示',
          desc: res.data.msg
        });
        // 如果直接返回res，则下一个新的promise，一定会直接执行onFulfilled
        // 返回一个Promise.resolve(res)或Promise.reject(res)，则新的promise的状态将由返回的状态决定
        return Promise.reject(res);
      }

      if (res.data.status < 0) {
        location.href = `#/login?returnUrl=${location.href}`;
        localStorage.removeItem('user');
        return Promise.reject(res);
      }
    }
  );
}
```

```javascript
// @/tool/Admin.js
import Axios from '@/tool/axios.js';
import {dateFormat} from '@/tool/transform.js';

export default class Admin {
  constructor(admin) {
    admin = admin || {};
    this.id = admin.id || -1;
    this.username = admin.username || '';
    this.password = admin.password || '';
  }

  login() {
    const that = this;
    return Axios({
      url: `/api/user/login`,
      method: 'put',
      isFormData: true,
      data: {
        username: that.username,
        password: that.password
      }
    }).then(
      res => Promise.resolve(res),
      err => Promise.reject(err)
    );
  }
}
```

```javascript
// login.vue
import Admin from '@/tool/Admin.js'
that.admin.login().then(
    res => {
        
    },
    err => {
        console.error(err);
    }
);
```

```javascript
 // 整体调用等同于
 axios(config).then().then().then();
 // 每一个.then()都返回一个新的promise
```

