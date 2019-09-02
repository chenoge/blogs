---
title: 严格模式
date: 2019-09-02 10:26:37
tags: [严格模式]
---

#### 严格模式

严格模式对`Javascript`的语法和行为，做了一些改变：

- **全局变量显式声明**：在正常模式中，如果一个变量没有声明就赋值，默认是全局变量。严格模式禁止这种用法，**全局变量必须显式声明**。

  ```javascript
  "use strict";
  
  v = 1; // 报错，v未声明
  for(i = 0; i < 2; i++) {}; // 报错，i未声明
  ```

<!--more-->



- **显式报错**：严格模式会使引起**静默失败**（不报错也没有任何效果）的**赋值**操作抛出异常。

  ```javascript
  "use strict";
  
  // 给不可写属性赋值
  var obj1 = {};
  Object.defineProperty(obj1, "x", { value: 42, writable: false });
  obj1.x = 9; // 抛出TypeError错误
  ```



- **禁止删除变量**：严格模式**禁止删除声明变量**。



- **禁止使用with语句**



- **创设`eval`作用域**：正常模式下，`eval`语句的作用域，取决于它处于**全局作用域**，还是处于**函数作用域**。严格模式下，**`eval`语句本身就是一个作用域**，不再能够生成全局变量了，它所生成的变量只能用于`eval`内部。

  ```javascript
  "use strict";
  var x = 2;
  console.info(eval("var x = 5; x")); // 5
  console.info(x); // 2
  ```



- **arguments对象的限制**：不允许对`arguments`赋值，不再追踪参数的变化，禁止使用`arguments.callee`。



- **重名错误**：严格模式下，对象不能有**重名的属性**，函数不能有**重名的参数**。

  ```javascript
  "use strict";
  var o = { p: 1, p: 2 }; // !!! 语法错误
  
  function sum(a, a, c) { // !!! 语法错误
    "use strict";
    return a + a + c; // 代码运行到这里会出错
  }
  ```



- **禁止八进制表示法**：正常模式下，整数的第一位如果是0，表示这是八进制数。严格模式禁止这种表示法，整数第一位为0，将报错。



- **保留字**：严格模式新增了一些保留字：`implements, interface, let, package, private, protected, public, static, yield`，使用这些词作为变量名将会报错。