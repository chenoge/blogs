---
title: 减少if-else和switch
date: 2018-10-10 23:01:31
tags: [if-else,switch]
---

[原文](https://juejin.im/post/5bb9e3085188255c352d7326)<br/>

## 1. 使用 Array.includes 来处理多重条件

举个栗子 🌰：

```javascript
// 条件语句
function test(fruit) {
  if (fruit == 'apple' || fruit == 'strawberry') {
    console.log('red');
  }
}
```

乍一看，这么写似乎没什么大问题。然而，如果我们想要匹配更多的红色水果呢，比方说『樱桃』和『蔓越莓』？我们是不是得用更多的 `||` 来扩展这条语句？

我们可以使用 `Array.includes`[(Array.includes)](https://link.juejin.im/?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fen-US%2Fdocs%2FWeb%2FJavaScript%2FReference%2FGlobal_Objects%2FArray%2Fincludes) 重写以上条件句。

```javascript
function test(fruit) {
  // 把条件提取到数组中
  const redFruits = ['apple', 'strawberry', 'cherry', 'cranberries'];
  if (redFruits.includes(fruit)) {
    console.log('red');
  }
}
```

我们把`红色的水果`（条件）都提取到一个数组中，这使得我们的代码看起来更加整洁。

<br/>

<!--more-->

## 2. 少写嵌套，尽早返回

让我们为之前的例子添加两个条件：

- 如果没有提供水果，抛出错误。
- 如果该水果的数量大于 10，将其打印出来。

```javascript
function test(fruit, quantity) {
  const redFruits = ['apple', 'strawberry', 'cherry', 'cranberries'];
  // 条件 1：fruit 必须有值
  if (fruit) {
    // 条件 2：必须为红色
    if (redFruits.includes(fruit)) {
      console.log('red');
      // 条件 3：必须是大量存在
      if (quantity > 10) {
        console.log('big quantity');
      }
    }
  } else {
    throw new Error('No fruit!');
  }
}

// 测试结果
test(null); // 报错：No fruits
test('apple'); // 打印：red
test('apple', 20); // 打印：red，big quantity
```

 遵循的一个总的规则是**当发现无效条件时尽早返回**

```javascript
/_ 当发现无效条件时尽早返回 _/
function test(fruit, quantity) {
  const redFruits = ['apple', 'strawberry', 'cherry', 'cranberries'];
  if (!fruit) throw new Error('No fruit!'); // 条件 1：尽早抛出错误
  if (!redFruits.includes(fruit)) return; // 条件 2：当 fruit 不是红色的时候，直接返回
  console.log('red');
  // 条件 3：必须是大量存在
  if (quantity > 10) {
    console.log('big quantity');
  }
}
```

<br/>

## 3. 相较于 switch，Map / Object 也许是更好的选择

让我们看下面的例子，我们想要根据颜色打印出各种水果：

```javascript
function test(color) {
  // 使用 switch case 来找到对应颜色的水果
  switch (color) {
    case 'red':
      return ['apple', 'strawberry'];
    case 'yellow':
      return ['banana', 'pineapple'];
    case 'purple':
      return ['grape', 'plum'];
    default:
      return [];
  }
}

//测试结果
test(null); // []
test('yellow'); // ['banana', 'pineapple']
```

 上面的代码看上去并没有错，但是它看上去很冗长。同样的结果可以通过对象字面量来实现，语法也更加简洁：

```javascript
// 使用对象字面量来找到对应颜色的水果
  const fruitColor = {
    red: ['apple', 'strawberry'],
    yellow: ['banana', 'pineapple'],
    purple: ['grape', 'plum']
  };

function test(color) {
  return fruitColor[color] || [];
} 
```

 或者，你也可以使用 [Map](https://link.juejin.im?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fen-US%2Fdocs%2FWeb%2FJavaScript%2FReference%2FGlobal_Objects%2FMap) 来实现同样的效果：

```javascript
// 使用 Map 来找到对应颜色的水果
  const fruitColor = new Map()
    .set('red', ['apple', 'strawberry'])
    .set('yellow', ['banana', 'pineapple'])
    .set('purple', ['grape', 'plum']);

function test(color) {
  return fruitColor.get(color) || [];
}
```

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 