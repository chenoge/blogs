---
title: JavaScript设计模式-装饰者模式
date: 2019-01-04 20:35:39
tags: [设计模式,装饰者模式]
---

#### 一、装饰者模式

**装饰者(decorator)**模式能够在不改变对象自身的基础上，在程序运行期间给对像动态的添加职责。与继承相比，装饰者是一种更轻便灵活的做法。

```
装饰者模式将一个对象嵌入到另一个对象之中，实际上相当于这个对象被另一个对像包装起来，形成一条包装链。
请求随着这条包装链依次传递到所有的对象，每个对象都有处理这条请求的机会。
```

<br/>



#### 二、装饰函数

在JavaScript中可以很方便的给某个对象扩展属性和方法，但却很难在不改动某个函数源代码的情况下，给该函数添加一些额外的功能。**也就是在代码运行期间，我们很难切入某个函数的执行环境**。 

##### 1、使用装饰者模式例子

```javascript
/对window.onload的处理

window.onload=function () {
    console.log('test');
};

var  _onload= window.onload || function () {};

window.onload=function () {
    _onload();
    console.log('自己的处理函数');
};
```

<br/>

##### 2、使用AOP（面向切面编程）装饰函数

在需要执行的函数之前执行某个新添加的功能函数

```javascript
// 封装的before函数
Function.prototype.before = function(beforefn) {
    var __self = this;
    return function() {
        beforefn.apply(this, arguments);
        return __self.apply(this, arguments);
    };
};
```
<br/>

<!--more-->

在需要执行的函数之后执行某个新添加的功能函数

```javascript
// 封装的after函数
Function.prototype.after = function( afterfn ){ 
    var __self = this;
    return function(){
        var ret = __self.apply( this, arguments ); 
        afterfn.apply( this, arguments );
        return ret;
    } 
};
```

<br/>

不污染Function原型的做法

```javascript
var before = function(fn, beforeFn) {
    return function() {
        beforeFn.apply(this, arguments);
        return fn.apply(this, arguments);
    };
};

function fn() { console.log('fn') }

function beforeFn() { console.log('beforeFn') }

fn = before(fn, beforeFn);

fn();
```

<br/>

##### 3、使用装饰者模式动态的改变ajax函数

```javascript
Function.prototype.before = function(beforefn) {
    var _this = this;
    return function() {
        beforefn.apply(this, arguments);
        return _this.apply(this, arguments);
    };
};

//给ajax请求动态添加参数的例子
var ajax = function(type, url, param) {
    console.log(param);
};

var getToken = function() {
    return 'Token';
};

ajax = ajax.before(function(type, url, param) {
    param.token = getToken();
});

ajax('get', 'http://www.jn.com', { name: 'zhiqiang' });
```

<br/>