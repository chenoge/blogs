---
title: 遍历器Iterator
date: 2019-03-06 15:47:45
tags: [遍历器,Iterator,for...of]
---

#### 可遍历

`JavaScript`中表示`集合`的**数据结构**，主要有数组（`Array`）、对象（`Object`）、`Map`、`Set` 等四种数据集合。一种**数据结构**只要部署了`Iterator` 接口，我们就称这种数据结构是`可遍历的`（iterable）。

<br/>

`Iterator`的遍历过程是这样的：

（1）创建一个指针对象，指向当前数据结构的起始位置。也就是说，遍历器对象本质上，就是一个指针对象。

（2）第一次调用指针对象的`next`方法，可以将指针指向数据结构的第一个成员。

（3）第二次调用指针对象的`next`方法，指针就指向数据结构的第二个成员。

（4）不断调用指针对象的`next`方法，直到它指向数据结构的结束位置。

<br/>

每一次调用`next`方法，都会返回数据结构的当前成员的信息。具体来说，就是返回一个包含`value`和`done`两个属性的对象。其中，`value`属性是当前成员的值，`done`属性是一个布尔值，表示遍历是否结束。 

```javascript
// 模拟next方法返回值的例子

var it = makeIterator(['a', 'b']);

it.next() // { value: "a", done: false }
it.next() // { value: "b", done: false }
it.next() // { value: undefined, done: true }

function makeIterator(array) {
  var nextIndex = 0;
  return {
    next: function() {
      return nextIndex < array.length ?
        {value: array[nextIndex++], done: false} :
        {value: undefined, done: true};
    }
  };
}
```
<!--more-->

<br/>

#### 默认的`Iterator`接口

`ES6 `规定，默认的 `Iterator` 接口部署在**数据结构**的`Symbol.iterator`属性。或者说，一个数据结构只要具有`Symbol.iterator`属性，就可以认为是**可遍历**的（`iterable`）。`Symbol.iterator`属性是一个**遍历器生成函数**，执行这个函数，就会返回一个**遍历器**。 

```javascript
// 对象obj是可遍历的（iterable），因为具有Symbol.iterator属性

const obj = {
  [Symbol.iterator] : function () {
    return {
      next: function () {
        return {
          value: 1,
          done: true
        };
      }
    };
  }
};
```

```javascript
Number.prototype[Symbol.iterator] = function() {
    return {
        v: 0,
        e: this,
        next() {
            return {
                value: this.v++,
                done: this.v > this.e
            }
        }
    }
}

[...100] // 返回[0, 1, 2, ..., 99]
```

原生具备 Iterator 接口的数据结构如下：

- Array
- Map
- Set
- String
- TypedArray
- 函数的 arguments 对象
- NodeList 对象

```javascript
let arr = ['a', 'b', 'c'];
let iter = arr[Symbol.iterator]();

iter.next() // { value: 'a', done: false }
iter.next() // { value: 'b', done: false }
iter.next() // { value: 'c', done: false }
iter.next() // { value: undefined, done: true }
```

注：ES6 创造了一种新的**遍历命令**`for...of`循环，`Iterator`接口主要供`for...of`消费

<br/>

#### 调用`Iterator `接口的场合

默认调用 Iterator 接口（即`Symbol.iterator`方法）的一些场合

- `for...of`循环

- 解构赋值

  ```javascript
  let set = new Set().add('a').add('b').add('c');
  
  let [x,y] = set;
  // x='a'; y='b'
  
  let [first, ...rest] = set;
  // first='a'; rest=['b','c'];
  ```

- 扩展运算符

  ```javascript
  // 例一
  var str = 'hello';
  [...str] //  ['h','e','l','l','o']
  
  // 例二
  let arr = ['b', 'c'];
  ['a', ...arr, 'd']
  // ['a', 'b', 'c', 'd']
  ```

- `yield* ` 

- 由于数组的遍历会调用遍历器接口，所以任何接受数组作为参数的场合，其实都调用了遍历器接口

<br/>

#### 遍历器对象的`return`方法 

`return`方法的使用场合是，如果`for...of`循环提前退出（通常是因为出错，或者有`break`语句），就会调用`return`方法，是可选的 。

```javascript
function readLinesSync(file) {
  return {
    [Symbol.iterator]() {
      return {
        next() {
          return {value:1, done: false };
        },
        return() {
          console.log(file);
          return {value:2, done: true };
        }
      };
    },
  };
}
```

```javascript
// 情况一：break语句
for (let line of readLinesSync(fileName)) {
  console.log(line);
  break;
}

// 情况二：出错
for (let line of readLinesSync(fileName)) {
  console.log(line);
  throw new Error();
}
```

注意：`return`方法必须返回一个对象

<br/>



