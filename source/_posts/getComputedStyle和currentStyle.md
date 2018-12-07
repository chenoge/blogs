---
title: getComputedStyle和currentStyle
date: 2018-12-07 15:39:03
tags: [getComputedStyle,currentStyle]
---

#### getComputedStyle

`Window.getComputedStyle()`方法返回一个对象，该对象在应用活动样式表并解析这些值可能包含的任何基本计算后报告元素的所有CSS属性的值。 私有的CSS属性值可以通过对象提供的API或通过简单地使用CSS属性名称进行索引来访问。 

```javascript
let style = window.getComputedStyle(element, [pseudoElt]);
// let afterStyle = window.getComputedStyle(h3, '::after');
```

- `pseudoElt` 可选，指定一个要匹配的**伪元素**的字符串。

<br/>

<!--more-->

### defaultView

在许多在线的演示代码中，`getComputedStyle`是通过 `document.defaultView` 对象来调用的。大部分情况下，这是不需要的，因为可以直接通过`window`对象调用。但有一种情况，你必需要使用 `defaultView`,  那是在`firefox3.6`上访问子框架内的样式 。 

<br/>

### currentStyle

`Element.currentStyle` 是一个与 `window.getComputedStyle`方法功能相同的属性。这个属性实现在旧版本的`IE`浏览器中。

