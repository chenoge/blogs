---
title: CommonJS/AMD/CMD区别
date: 2018-11-14 17:28:02
tags: [CommonJS,AMD,CMD]
---

### 一、概括

`CommonJs`用在服务器端，`AMD`和`CMD`用在浏览器环境。`AMD` 是 `RequireJS` 在推广过程中对模块定义的规范化产出。`CMD` 是 `SeaJS` 在推广过程中对模块定义的规范化产出。

<br/>

<!--more-->

### 二、CommonJs

`CommonJS`是服务器端模块的规范，由`Node`推广使用。由于服务端编程的复杂性，如果没有模块很难与操作系统及其他应用程序互动。使用方法如下： `module.exports`和`require`

```javascript
// math.js
module.exports.add = function() {
    var sum = 0, i = 0, args = arguments, l = args.length;
    while (i < l) {
      sum += args[i++];
    }
    return sum;
};


// increment.js
var add = require('math').add;
module.exports.increment = function(val) {
    return add(val, 1);
};


// index.js
var increment = require('increment').increment;
var a = increment(1); //2
```

注：`CommonJs`中的`require `是同步的 

<br/>

### 三、AMD

`AMD`（`Asynchronous Module Definition`），意思就是"异步模块定义"。由于不是`JavaScript`原生支持，使用`AMD`规范进行页面开发需要用到对应的库函数，也就是大名鼎鼎`RequireJS`，实际上`AMD` 是 `RequireJS` 在推广过程中对模块定义的规范化的产出。

它采用异步方式加载模块，模块的加载不影响它后面语句的运行。所有依赖这个模块的语句，都定义在一个回调函数中，等到加载完成之后，这个回调函数才会运行。

#### 1、define()函数

RequireJS定义了一个函数 define，它是全局变量，用来定义模块: 

```javascript
define(id?, dependencies?, factory);
```

id：指定义中模块的名字，可选；如果没有提供该参数，模块的名字应该默认为模块加载器请求的指定脚本的名字。如果提供了该参数，模块名必须是“顶级”的和绝对的（不允许相对名字）。 

dependencies：是一个当前模块依赖的，已被模块定义的模块标识的数组字面量。 依赖参数是可选的，如果忽略此参数，它应该默认为["require", "exports", "module"]。然而，如果工厂方法的长度属性小于3，加载器会选择以函数的长度属性指定的参数个数调用工厂方法。 

factory：模块初始化要执行的函数或对象。如果为函数，它应该只被执行一次。如果是对象，此对象应该为模块的输出值。 

```javascript
define("alpha", ["require", "exports", "beta"], function (require, exports, beta) {

       exports.verb = function() {

           return beta.verb();

           //Or:

           return require("beta").verb();

       }
});
```

#### 2、RequireJs使用

```javascript
// main.js

//别名配置
requirejs.config({
    paths: {
        jquery: 'jquery.min' //可以省略.js
    }
});

//引入模块，用变量$表示jquery模块
requirejs(['jquery'], function ($) {
    $('body').css('background-color','red');
});
```

```javascript
// math.js
define('math',['jquery'], function ($) {//引入jQuery模块
    return {
        add: function(x,y){
            return x + y;
        }
    };
});

// main.js
require(['jquery','math'], function ($,math) {
    console.log(math.add(10,100));//110
});
```

<br/>

### 四、CMD

`CMD` 即`Common Module Definition`通用模块定义，`CMD`规范是国内发展出来的，就像`AMD`有个`requireJS`，`CMD`有个浏览器的实现`SeaJS`，`SeaJS`要解决的问题和`requireJS`一样，只不过在模块定义方式和模块加载（可以说运行、解析）时机上有所不同。 

```javascript
define(function(require, exports, module) {
  // 模块代码
});
// require是可以把其他模块导入进来的一个参数;
// 而exports是可以把模块内的一些属性和方法导出的;
// module 是一个对象，上面存储了与当前模块相关联的一些属性和方法。
```

```javascript
// 定义模块  myModule.js
define(function(require, exports, module) {
  var $ = require('jquery.js')
  $('p').addClass('active');
  exports.data = 1;
});

// 加载模块
seajs.use(['myModule.js'], function(my){
    var star= my.data;
    console.log(star);  //1
});
```

