---
title: 拦截全局ajax请求与Ajax-hook源码分析
date: 2018-07-03 19:35:35
tags: [ajax,hook,拦截请求]
---

# 一、[ajax-hook插件](https://github.com/wendux/Ajax-hook)

#### 原理图

![image](拦截全局ajax请求与Ajax-hook源码分析\ajaxhook.png)

<br/>

<!--more-->

#### 用法

- 引入文件

  - Using cdn

    ```javascript
    <script src="https://unpkg.com/ajax-hook/dist/ajaxhook.min.js"></script>
    ```

  - Using npm

    ```npm
    npm install ajax-hook
    ```

- 注册回调函数

  - ```javascript
    hookAjax({
      //hook callbacks
      onreadystatechange:function(xhr){
        console.log("onreadystatechange called: %O",xhr)
      },
      onload:function(xhr){
        console.log("onload called: %O",xhr)
      },
      //hook function
      open:function(arg,xhr){
        console.log("open called: method:%s,url:%s,async:%s",arg[0],arg[1],arg[2])
      }
    })
    
    // NPM
    // const ah=require("ajax-hook")
    // ah.hookAjax({...})
    ```

- 异步请求

  - ```javascript
    // get current page source code 
    $.get().done(function(d){
        console.log(d.substr(0,30)+"...")
    })
    ```

- 结果

  - ```javascript
    > open called: method:GET,url:http://localhost:63342/Ajax-hook/demo.html,async:true
    > onload called: XMLHttpRequest
    > <!DOCTYPE html>
      <html>
      <head l...
    ```

<br/>

# 二、源码

```javascript
// index.js
require("./src/ajaxhook")(module.exports)
```

```javascript
// /src/ajaxhook.js

/*
 * author: wendu
 * email: 824783146@qq.com
 * source code: https://github.com/wendux/Ajax-hook
 **/

// ob是index.js传入的module.exports
module.exports=function (ob) {
    ob.hookAjax = function (funs) {
        window._ahrealxhr = window._ahrealxhr || XMLHttpRequest
        XMLHttpRequest = function () {
            // this是new XMLHttpRequest()返回的对象
            this.xhr = new window._ahrealxhr;
            // 复制this.xhr的属性到this对象
            for (var attr in this.xhr) {
                var type = "";
                try {
                    type = typeof this.xhr[attr]
                } catch (e) {}
                if (type === "function") {
                    // hookfun为函数调用，this指向window（非严格模式）
                    this[attr] = hookfun(attr);
                } else {
                    Object.defineProperty(this, attr, {
                        get: getFactory(attr),
                        set: setFactory(attr)
                    })
                }
            }
        }

        function getFactory(attr) {
            return function () {
                return this.hasOwnProperty(attr + "_")?this[attr + "_"]:this.xhr[attr];
            }
        }

        function setFactory(attr) {
            return function (f) {
                var xhr = this.xhr;
                var that = this;
                if (attr.indexOf("on") != 0) {
                    this[attr + "_"] = f;
                    return;
                }
                if (funs[attr]) {
                    xhr[attr] = function () {
                        funs[attr](that) || f.apply(xhr, arguments);
                    }
                } else {
                    xhr[attr] = f;
                }
            }
        }

        function hookfun(fun) {
            // console.log(this)
            // this指向hookfun调用者，此处为window
            return function () {
                var args = [].slice.call(arguments)
                // 函数方法是否在注册回调函数的列表中
                if (funs[fun] && funs[fun].call(this, args, this.xhr)) {
                    return;
                }
                // 匿名函数的调用者为new XMLHttpRequest()对象
                return this.xhr[fun].apply(this.xhr, args);
            }
        }
        return window._ahrealxhr;
    }
    
    // 恢复XMLHttpRequest对象
    ob.unHookAjax = function () {
        if (window._ahrealxhr)  XMLHttpRequest = window._ahrealxhr;
        window._ahrealxhr = undefined;
    }
    //for typescript
    ob.default=ob;
}
```

