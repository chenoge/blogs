---
title: JS-valueOf与toString
date: 2017-10-29 18:50:48
tags: [valueOf,toString]
---

# 一、抛题

实现一个函数，运算结果可以满足如下预期结果：

```javascript
add(1)(2) // 3
add(1, 2, 3)(10) // 16
add(1)(2)(3)(4)(5) // 15
```

<br/>

<!--more-->

# 二、破题

难点：如何既返回一个值又返回一个函数以供后续继续调用？

```javascript
function  add ()  {
    var  args  =  Array.prototype.slice.call(arguments);
    var  fn  =   function ()  {
        var  arg_fn  =  Array.prototype.slice.call(arguments);
        return  add.apply(null,  args.concat(arg_fn));
    }
    fn.valueOf  =   function ()  {
        return  args.reduce(function(a,  b)  {
            return  a  +  b;        
        })    
    }     
    return  fn;
}
```

<br/>

# 三、解题

#### 1、Object.prototype.valueOf()

JavaScript 调用 valueOf() 方法用来把对象转换成原始类型的值（数值、字符串和布尔值）。但是我们很少需要自己调用此函数，valueOf 方法一般都会被 JavaScript 自动调用。

 

#### 2、Object.prototype.toString()

每个对象都有一个 toString() 方法，当对象被表示为文本值时或者当以期望字符串的方式引用对象时，该方法被自动调用。



#### 3、类型的转换规则 

##### (1)String 类型转换 

转换规则：

1. 如果 toString 方法存在并且返回原始类型，返回      toString 的结果
2. 如果      toString 方法不存在或者返回的不是原始类型，调用 valueOf 方法
3. 如果      valueOf 方法存在，并且返回原始类型数据，返回 valueOf 的结果
4. 其他情况，抛出错误

```javascript
var obj = {
    toString: function() {
        console.log('调用了 obj.toString');
        return {};
    },
    valueOf: function() {
        console.log('调用了 obj.valueOf')
        return '110';
    }
}
alert(obj);
// 调用了 obj.toString
// 调用了 obj.valueOf
// 110
```

<br/>

##### (2)Number 类型转换 

转换规则：

1. 如果 valueOf 存在，且返回原始类型数据，返回 valueOf 的结果。
2. 如果      toString 存在，且返回原始类型数据，返回 toString 的结果。
3. 其他情况，抛出错误。

```javascript
var obj = {
    valueOf: function() {
        console.log('调用 valueOf');
        return {};
    },
    toString: function() {
        console.log('调用 toString');
        return 10;
    }
}

console.log(obj + 1);
// 调用 valueOf
// 调用 toString
// 11
```

<br/>

##### (3)Boolean 类型转换 

转换规则

- 除了下述 6 个值转换结果为 false，其他全部为 true：
  - undefined
  - null
  - -0
  - 0或+0
  - NaN
  - ”（空字符串）

<br/>

##### (4)Function类型转换 

规则转换

1. 如果 valueOf 存在，且返回原始类型数据，返回 valueOf 的结果。
2. 如果      toString 存在，且返回原始类型数据，返回 toString 的结果。
3. 其他情况，抛出错误。

```javascript
test.valueOf = function() {
    console.log('调用 valueOf 方法');
    return {};
}

test.toString= function() {
    console.log('调用 toString 方法');
    return 3;
}
		 
test;
// 输出如下：
// 调用 valueOf 方法
// 调用 toString 方法
// 3
```

