---
title: JS-开放封闭原则
date: 2018-04-29 17:15:47
tags: [javaScript,原则]
---

# 一、两个主要特性

- 它们 "面向**扩展开放**（Open For Extension）"。
  - 模块的行为是能够被扩展的。当应用程序的需求变化时，我们可以使模块表现出全新的或与以往不同的行为，以满足新的需求。



- 它们 "面向**修改封闭**（Closed For Modification）"。
  - 模块的源代码是不能被侵犯的，任何人都不允许修改已有源代码。



```javascript
  //检测字符串
  //checkType('165226226326','mobile')

  let checkType = function(str, type) {
      switch (type) {
          case 'email':
              return /^[\w-]+(\.[\w-]+)*@[\w-]+(\.[\w-]+)+$/.test(str);
          case 'mobile':
              return /^1[3|4|5|7|8][0-9]{9}$/.test(str);
          case 'tel':
              return /^(0\d{2,3}-\d{7,8})(-\d{1,4})?$/.test(str);
          case 'number':
              return /^[0-9]$/.test(str);
          case 'english':
              return /^[a-zA-Z]+$/.test(str);
          case 'text':
              return /^\w+$/.test(str);
          case 'chinese':
              return /^[\u4E00-\u9FA5]+$/.test(str);
          case 'lower':
              return /^[a-z]+$/.test(str);
          case 'upper':
              return /^[A-Z]+$/.test(str);
          default:
              return true;
      }
  }
```

这个 API 看着没什么毛病，能检测常用的一些数据。但是有以下两个问题。

 

1. 但是如果想到添加其他规则的呢？就得在函数里面增加 case 。添加一个规则就修改一次！这样违反了开放-封闭原则（对扩展开放，对修改关闭）。而且这样也会导致整个 API 变得臃肿，难维护。

 

2. 还有一个问题就是，比如A页面需要添加一个金额的校验，B页面需要一个日期的校验，但是金额的校验只在A页面需要，日期的校验只在B页面需要。如果一直添加 case 。就是导致A页面把只在B页面需要的校验规则也添加进去，造成不必要的开销。B页面也同理。



建议的方式是给这个 API 增加一个扩展的接口

```javascript
let checkType = (function() {
    let rules = {
        email(str) {
            return /^[\w-]+(\.[\w-]+)*@[\w-]+(\.[\w-]+)+$/.test(str);
        },
        mobile(str) {
            return /^1[3|4|5|7|8][0-9]{9}$/.test(str);
        },
        tel(str) {
            return /^(0\d{2,3}-\d{7,8})(-\d{1,4})?$/.test(str);
        },
        number(str) {
            return /^[0-9]$/.test(str);
        },
        english(str) {
            return /^[a-zA-Z]+$/.test(str);
        },
        text(str) {
            return /^\w+$/.test(str);
        },
        chinese(str) {
            return /^[\u4E00-\u9FA5]+$/.test(str);
        },
        lower(str) {
            return /^[a-z]+$/.test(str);
        },
        upper(str) {
            return /^[A-Z]+$/.test(str);
        }
    };

    //暴露接口
    return {
        //校验
        check(str, type) {
            return rules[type] ? rules[type](str) : false;
        },
        //添加规则
        addRule(type, fn) {
            rules[type] = fn;
        }
    }

})();



//调用方式
//使用mobile校验规则
console.log(checkType.check('188170239', 'mobile'));

//添加金额校验规则
checkType.addRule('money', function(str) {
    return /^[0-9]+(.[0-9]{2})?$/.test(str)
});

//使用金额校验规则
console.log(checkType.check('18.36', 'money'));
```

