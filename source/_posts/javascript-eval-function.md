---
title: javascript-eval-function
date: 2019-09-02 10:26:37
tags: [Function,eval]
---

#### `Function`

**Function 构造函数**创建一个新的`Function`对象。 在 JavaScript 中, 每个函数实际上都是一个`Function`对象。

```
let fn = new Function ([arg1[, arg2[, ...argN]],] functionBody);
```

```javascript
let sum = new Function('a', 'b', 'return a + b');
console.log(sum(2, 6)); // 8
```

<!--more-->

注：

- **以调用函数的方式**调用**Function 构造函数**，跟以构造函数来调用是一样的

- 使用**Function 构造函数**生成的`Function`对象是**在函数创建时解析**的。这比使用**函数声明**或者**函数表达式**并在代码中调用更为低效，因为使用后者创建的函数是跟其他代码一起解析的。

- 使用**Function 构造函数**生成的函数，并不会在**创建它们的上下文**中创建闭包，它们一般**在全局作用域中被创建**。当运行这些函数的时候，它们只能访问自己的本地变量和全局变量，不能访问**Function 构造函数**被调用生成的上下文的作用域。这和使用带有函数表达式代码的 `eval` 不同。

  ```javascript
  // 1、f()函数返回的function e()是闭包.
  var n = 1;
  function f(){
      var n = 2;
      function e(){
          return n;
      }
      return e;
  }
  console.log (f()()); // 2
  ```

  ```javascript
  // 2、f()函数返回的function e()是全局作用域函数
  var n = 1;
  function f(){
      var n = 2;
      var e = new Function("return n;");
      return e;
  }
  console.log (f()()); // 1
  ```



#### `eval`

`eval()` 函数会将传入的字符串当做 `JavaScript` 代码进行执行。

```
var value = eval(string);
```

返回值：

- 返回**字符串代码的返回值**，如果返回值为空，则返回 `undefined`

- 如果 `eval()` 的参数不是字符串， `eval()` 会将参数原封不动地返回

注意：

- 如果你间接的使用 `eval()`，而不是直接的调用 `eval`。 从 `ECMAScript 5`起，它工作在全局作用域下，而不是局部作用域中。

  ```javascript
  function test() {
      var x = 2, y = 4;
      // 直接调用，使用本地作用域
      console.log(eval('x + y'));  // 6
      
      // 等价于在全局作用域调用
      var geval = eval; 
      
      // 间接调用，使用全局作用域
      console.log(geval('x + y')); // throws ReferenceError 因为`x`未定义
      
       // 另一个间接调用的例子
      (0, eval)('x + y');
  }
  ```

- `eval` 中函数作为字符串被定义需要`（`和`）`作为前缀和后缀

  ```javascript
  let fctStr1 = 'function a() {}'
  let fctStr2 = '(function a() {})'
  let fct1 = eval(fctStr1)  // 返回undefined
  let fct2 = eval(fctStr2)  // 返回一个函数
  ```

  