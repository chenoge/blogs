---
title: JS-属性描述符
date: 2018-01-20 21:29:40
tags: [javaScript,属性描述符]
---

# 一、属性描述符

ECMAScript对象中目前存在的`属性描述符`主要有两种，`数据描述符`(数据属性)和`存取描述符`(访问器属性)。

- `数据描述符`是一个拥有可写或不可写值的属性。
- `存取描述符`是由一对 getter-setter 函数功能来描述的属性。 

<br/>

# 二、数据描述符属性

当修改或定义对象的某个属性的时候，给这个属性添加一些特性： 

```javascript
Object.defineProperty(obj, "newKey", {
    configurable: true | false,
    enumerable: true | false,
    value: 任意类型的值,
    writable: true | false
});
```

- **Value**：属性对应的值,可以使任意类型的值，默认为undefined
- **Writable**：属性的值是否可以被重写，默认为false。
- **Enumerable**：此属性是否可以被枚举，默认为false。
- **Configurable**：设置为true可以被删除或可以重新设置特性，默认为false。

提示：一旦使用Object.defineProperty给对象添加属性，那么如果不设置属性的特性，那么`configurable`、`enumerable`、`writable`这些值都为默认的`false `

<br/>

```javascript
Object.defineProperty(person, 'name', {
    configurable: false,
    value: 'John'
});
```

<br/>

# 三、存取描述符属性

当使用存取器描述属性的特性的时候，允许设置以下特性属性： 

```javascript
var obj = {};
Object.defineProperty(obj, "newKey", {
    get: function() {} | undefined,
    set: function(value) {} | undefined
    configurable: true | false
    enumerable: true | false
});
```

注：

- 当使用了getter或setter方法，不允许使用writable和value这两个属性 
- get或set不是必须成对出现，任写其一就可以。如果不设置方法，则get和set的默认值为undefined

<br/>

```javascript
Object.defineProperty(obj, 'a', {
    configurable: true,
    enumerable: true,
    get: function() {
        return aValue
    },
    set: function(newValue) {
        aValue = newValue;
        b = newValue + 1
    }
})
```

<br/>

# 四、Object.defineProperties 

```javascript
var obj = new Object();
Object.defineProperties(obj, {
    name: {
        value: '张三',
        configurable: false,
        writable: true,
        enumerable: true
    },
    age: {
        value: 18,
        configurable: true
    }
})
console.log(obj.name, obj.age) // 张三, 18
```

