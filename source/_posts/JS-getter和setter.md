---
title: JS-getter和setter
date: 2018-01-20 21:43:57
tags: [javaScript,getter,setter]
---

# 一、getter

```javascript
{get prop() { ... } }
 
{get [expression]() { ... } }
```

```javascript
var obj = {
  log: ['example','test'],
  get latest() {
      return this.log[this.log.length - 1];
  }
}
console.log(obj.latest); // "test"

```

<br/>

# 二、setter

```javascript
{set prop(val) { . . . }}

{set [expression](val) { . . . }}
```

```javascript
var language = {
    set current(name) {
        this.log.push(name);
    },
    log: []
}
language.current = 'EN';
console.log(language.log); // ['EN']
```

<br/>

# 三、注意

1、不能将一个 getter或setter 绑定到一个具有真实值的属性上

2、使用delete操作符可以删除getter和setter 





