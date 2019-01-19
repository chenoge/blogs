---
title: Proxy
date: 2019-01-19 18:54:15
tags: [Proxy,代理]
---

#### 一、语法

```javascript
var p = new Proxy(target, handler);
```

##### 传参说明

- `target`：使用`Proxy`包装的目标对象，它可以是任何类型的对象，包括数组、函数或甚至另一个`proxy`对象
- `handler`：一个对象，其属性是在对其执行操作时定义代理行为的函数

##### 对象代理

```javascript
var obj = new Proxy({}, {
    get: function(target, key, receiver) {
        console.log(`getting ${key}!`);
        return Reflect.get(target, key, receiver);
    },
    set: function(target, key, value, receiver) {
        console.log(`setting ${key}!`);
        return Reflect.set(target, key, value, receiver);
    }
});
```

注：

- 要使得`Proxy`起作用，**必须针对`Proxy`实例进行操作**，而不是针对目标对象进行操作 
- 如果`handler`没有设置任何拦截，那就等同于**直接通向原对象** 
- Proxy 实例可以**作为其他对象的原型对象** 

##### 函数代理

```javascript
var handler = {
    get: function(target, name) {
        if (name === 'prototype') {
            return Object.prototype;
        }
        return 'Hello, ' + name;
    },

    apply: function(target, thisBinding, args) {
        return args[0];
    },

    construct: function(target, args) {
        return { value: args[1] };
    }
};

var fproxy = new Proxy(function(x, y) {
    return x + y;
}, handler);

fproxy(1, 2) // 1
new fproxy(1, 2) // {value: 2}
fproxy.prototype === Object.prototype // true
fproxy.foo === "Hello, foo" // true
```

注：`Proxy`比`Object.defineProperty` 的功能更强大。Vue的作者宣称将在Vue3.0版本后加入`Proxy`从而代替`Object.defineProperty` 。

<!--more-->

<br/>

#### 二、创建可撤销的Proxy对象 

```javascript
// 创建
var revocable = Proxy.revocable(target, handler);
// 返回一个包含了所生成的代理对象本身以及该代理对象的撤销方法的对象
// 其结构为： {"proxy": proxy, "revoke": revoke}
```

##### 返回对象参数说明

- `proxy`：表示新生成的代理对象本身，和用一般方式 `new Proxy(target, handler)` 创建的代理对象没什么不同，只是它可以被撤销掉。
- `revoke`：撤销方法，调用的时候不需要加任何参数，就可以撤销掉和它一起生成的那个代理对象。

<br/>

注：一旦某个代理对象被撤销，它将变的几乎完全不可用，永远不可能恢复到原来的状态，在它身上执行任何的**可代理操作**都会抛出 [`TypeError`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/TypeError) 异常。同时和它关联的**目标对象**以及**处理器对象**将有可能被垃圾回收掉。

```javascript
var revocable = Proxy.revocable({}, {
  get(target, name) {
    return "[[" + name + "]]";
  }
});
var proxy = revocable.proxy;
proxy.foo;              // "[[foo]]"

revocable.revoke();     // 执行撤销方法

proxy.foo;              // TypeError
proxy.foo = 1           // 同样 TypeError
delete proxy.foo;       // 还是 TypeError
typeof proxy            // "object"，因为 typeof 不属于可代理操作
```

<br/>

#### 三、13 种Proxy代理操作

- **get(target, propKey, receiver)**：拦截对象属性的读取，比如`proxy.foo`和`proxy['foo']` 



- **set(target, propKey, value, receiver)**：拦截对象属性的设置，比如`proxy.foo = v`或`proxy['foo'] = v`，返回一个布尔值 



- **has(target, propKey)**：拦截`propKey in proxy`的操作，返回一个布尔值



- **deleteProperty(target, propKey)**：拦截`delete proxy[propKey]`的操作，返回一个布尔值



- **ownKeys(target)**：拦截`Object.getOwnPropertyNames(proxy)`、`Object.getOwnPropertySymbols(proxy)`、`Object.keys(proxy)`、`for...in`循环，返回一个数组。该方法返回目标对象所有自身的属性的属性名，而`Object.keys()`的返回结果仅包括目标对象自身的可遍历属性



- **getOwnPropertyDescriptor(target, propKey)**：拦截`Object.getOwnPropertyDescriptor(proxy, propKey)`，返回属性的描述对象



- **defineProperty(target, propKey, propDesc)**：拦截`Object.defineProperty(proxy, propKey, propDesc）`、`Object.defineProperties(proxy, propDescs)`，返回一个布尔值



- **preventExtensions(target)**：拦截`Object.preventExtensions(proxy)`，返回一个布尔值



- **getPrototypeOf(target)**：拦截`Object.getPrototypeOf(proxy)`，返回一个对象



- **isExtensible(target)**：拦截`Object.isExtensible(proxy)`，返回一个布尔值



- **setPrototypeOf(target, proto)**：拦截`Object.setPrototypeOf(proxy, proto)`，返回一个布尔值。如果目标对象是函数，那么还有两种额外操作可以拦截



- **apply(target, object, args)**：拦截 Proxy 实例作为函数调用的操作，比如`proxy(...args)`、`proxy.call(object, ...args)`、`proxy.apply(...)`



- **construct(target, args)**：拦截 Proxy 实例作为构造函数调用的操作，比如`new proxy(...args)` 

<br/>

