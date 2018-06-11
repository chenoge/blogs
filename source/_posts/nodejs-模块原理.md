---
title: nodejs-模块原理
date: 2017-08-05 14:52:33
tags: [nodejs,模块]
---

# 零、区别

Node.js 模块遵循**CommonJS规范**，ES6 模块与CommonJS模块的差异有以下两点 

- CommonJS 模块输出的是一个**值的拷贝**，ES6      模块输出的是**值的引用**。
- CommonJS 模块是**运行时加载**，ES6      模块是**编译时输出接口**。

<br/>

# 一、Node.js 模块实现原理

 <font color=#A52A2A size=4 >**实现“模块”功能的奥妙就在于JavaScript是一种函数式编程语言，它支持闭包。**</font>如果我们把一段JavaScript代码用一个函数包装起来，这段代码的所有“全局”变量就变成了函数内部的局部变量。假如我们编写的hello.js代码是这样的：

```javascript
var s = 'Hello';
var name = 'world';
console.log(s + ' ' + name + '!');
```

Node.js加载了hello.js后，它可以把代码包装一下，变成这样执行： 

```javascript
(function () {
    // 读取的hello.js代码:
    var s = 'Hello';
    var name = 'world';
    console.log(s + ' ' + name + '!');
    // hello.js代码结束
})();
```

所以，Node利用JavaScript的函数式编程的特性，轻而易举地实现了模块的隔离。  但是，模块的输出module.exports怎么实现？这个也很容易实现，**Node可以先准备一个对象module**：

```javascript
// 准备module对象:
var module = {
    id: 'hello',
    exports: {}
};
	
var load = function (module) {
    // 读取的hello.js代码:
    function greet(name) {
        console.log('Hello, ' + name + '!');
    }
    module.exports = greet;
    // hello.js代码结束
    return module.exports;
};
	
var exported = load(module);
// 保存module:
save(module, exported);

```

可见，变量module是Node在加载js文件前准备的一个变量，并将其传入加载函数，我们在hello.js中 <font color=#A52A2A size=4 >**可以直接使用变量module原因就在于它实际上是函数的一个参数**</font>： 

```javascript
module.exports = greet;
```

通过把参数module传递给load()函数，hello.js就顺利地把一个变量传递给了Node执行环境，**Node会把module变量保存到某个地方**。 

由于Node保存了所有导入的module，当我们用require()获取module时，Node找到对应的module，把这个module的exports变量返回，这样，另一个模块就顺利拿到了模块的输出： 

```javascript
var greet = require('./hello');
```

以上是Node实现JavaScript模块的一个简单的原理介绍。 