---
title: JS-作用域链与闭包
date: 2018-05-17 12:10:42
tags: [javaScript,作用域,闭包]
---

# 一、执行环境(Execution Context)

每一个执行环境都关联了一个**变量对象(variable Object）或者活动对象（activation object）**。环境中定义的所有变量和函数都保存在这个对象中。**每个函数运行时都会产生一个执行环境。活动对象是一种特殊的变量对象。**

<br/>

**全局执行环境关联的是<font color=#A52A2A size=4 >变量对象</font>，函数执行环境关联的是<font color=#A52A2A size=4 >活动对象</font>。**可以将执行环境看作是一个对象：

``` javascript
EC = {
    VO: { /*执行环境关联的变量对象（variable object）*/ }
    this: {},
    Scope: { /*作用域链*/ }
}
```

<br/>

# 二、执行环境栈

当一个函数被调用时，函数执行环境就被压入一个环境栈中。而在函数执行之后，栈将该函数执行环境弹出，把控制权交给之前的执行环境。  举个例子： 

```javascript
let scope = "global";

function fn1() {
    return scope;
}

function fn2() {
    return scope;
}

fn1();

fn2();
```

上面代码执行情况演示：  

![执行环境栈](JS-作用域链\20170429211440774.png)

<br/>

# 三、作用域链

当某个函数第一次被调用时，就会**创建一个执行环境(execution context)以及相应的作用域链，并把作用域链赋值给一个特殊的内部属性([scope])**。然后**使用this，arguments和其他命名参数的值来初始化函数的活动对象(activation object)**。当前执行环境的变量对象始终在作用域链的第0位。<br/>

以上面的代码为例，当第一次调用fn1()时的作用域链如下图所示：

**（因为fn2()还没有被调用，所以没有fn2的执行环境）** 

![作用域链1](JS-作用域链\20170430104545087.png)

可以看到fn1活动对象里并没有scope变量，于是沿着作用域链(scope chain)向后寻找，结果在全局变量对象里找到了scope，所以就返回全局变量对象里的scope值。 



<font color=#A52A2A size=4 >**标识符解析是沿着作用域链一级一级地搜索标识符地过程**</font>。**搜索过程始终从作用域链地前端开始，然后逐级向后回溯，直到找到标识符为止（如果找不到标识符，通常会导致错误发生）—-《JavaScript高级程序设计》** 



再来看一段代码： 

``` javascript
function outer() {
    let scope = "outer";

    function inner() {
        return scope;
    }

    return inner;
}

let fn = outer();
fn();
```



outer()内部返回了一个inner函数，当调用outer时，<font color=#A52A2A size=4 >**inner函数的作用域链就已经被初始化了（复制父函数的作用域链，再在前端插入自己的活动对象）**</font>，具体如下图：  



![作用域链2](JS-作用域链\20170430112410039.png)

 一般来说，当某个环境中的所有代码执行完毕后，该环境被销毁（弹出环境栈），保存在其中的所有变量和函数也随之销毁。但是像上面那种有内部函数的又有所不同，**当outer()函数执行结束，<font color=#A52A2A size=4 >执行环境被销毁</font>，但是其<font color=#A52A2A size=4 >关联的活动对象并没有随之销毁</font>，而是一直存在于内存中**，因为该活动对象被其内部函数的作用域链所引用。  

<br/>

具体如下图： 

1. outer执行结束，内部函数开始被调用 。
2. outer执行环境等待被回收，outer的作用域链对全局变量对象和outer的活动对象引用都断了 。

![作用域链3](JS-作用域链\20170430115351877.png)

像上面这种内部函数的作用域链仍然保持着对父函数活动对象的引用，就是**闭包(closure)** 。