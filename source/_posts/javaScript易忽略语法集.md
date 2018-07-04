---
title: javaScript易忽略语法集
date: 2018-07-04 11:49:41
tags: [javaScript]
---

#### 箭头函数与this

箭头函数体内的`this`对象，就是定义时所在的对象，而不是使用时所在的对象 。

`this`指向的固定化，并不是因为箭头函数内部有绑定`this`的机制，实际原因是**箭头函数根本没有自己的`this`，导致内部的`this`就是外层代码块的`this`。**正是因为它没有`this`，所以也就不能用作构造函数。 箭头函数转成 ES5 的代码如下 

```javascript
// ES6
function foo() {
  setTimeout(() => {
    console.log('id:', this.id);
  }, 100);
}

// ES5
function foo() {
  var _this = this;
  setTimeout(function () {
    console.log('id:', _this.id);
  }, 100);
}
```

<br/>

<!--more-->

#### 顶层对象的属性与全局变量 

顶层对象的属性与全局变量挂钩，被认为是 JavaScript 语言最大的设计败笔之一。 

```java
window.a = 1;
a // 1

a = 2;
window.a // 2
```

ES6 为了改变这一点，一方面规定，为了保持兼容性，`var`命令和`function`命令声明的全局变量，依旧是顶层对象的属性；另一方面规定，**`let`命令、`const`命令、`class`命令声明的全局变量，不属于顶层对象的属性。**也就是说，从 ES6 开始，全局变量将逐步与顶层对象的属性脱钩。 

```javascript
var a = 1;
// 如果在 Node 的 REPL 环境，可以写成 global.a
// 或者采用通用方法，写成 this.a
window.a // 1

let b = 1;
window.b // undefined
```

<br/>

#### for与let

```javascript
// es6
var a = [];
for (let i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[6](); // 6
// 变量i是let声明的，当前的i只在本轮循环有效，所以每一次循环的i其实都是一个新的变量


//babel es5
"use strict";
var a = [];
var _loop = function _loop(i) {
  a[i] = function () {
    console.log(i);
  };
};
for (var i = 0; i < 10; i++) {
  _loop(i);
}
a[6](); // 6
```

<br/>

#### `for`循环父作用域 

```javascript
for (let i = 0; i < 3; i++) {
  let i = 'abc';
  console.log(i);
}
// abc
// abc
// abc
```

- **设置循环变量**的那部分是一个**父作用域**
- **循环体内部**是一个单独的**子作用域**

<br/>

#### 数组

##### 1、性质

- `javaScript`将指定的**数字索引值**转换成**字符串**，然后将其作为**属性名**来使用（索引范围：`0~(2^32)-2`）
- 所有的数组都是对象，可以为其创建任意名字的属性。
- 如果使用了负数或非整数，数值转换为字符串，字符串作为**属性名**来用。
- 如果使用了非负整数，数值转换为字符串，字符串当做**数组索引**，而非对象属性。

##### 2、特殊行为

- 如果为一个数组元素赋值，它的索引`i`大于或等于现有数组的长度时，length属性的值将设置为`i+1`
- 设置length属性为一个小于当前长度的非负整数`n`时，当前数组中那些索引值大于或者等于`n`的元素将从中删除。

<br/>

#### 函数

##### 1、调用方式

- 作为函数
- 作为方法
- 作为构造函数
- 通过`call()`和`apply()`方法间接调用

##### 2、特性

- 一条**函数声明语句**实际上**声明了一个变量**，并把一个函数对象赋值给它。而定义**函数表达式**时并**没有声明一个变量**。
- 如果**函数表达式**包含名称，函数的局部作用域将会包含一个绑定到**函数对象**的名称。
- **如果嵌套函数作为函数调用，其this值不是全局对象就是undefined。**

<br/>

