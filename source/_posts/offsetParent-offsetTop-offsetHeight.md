---
title: offsetParent-offsetTop-offsetHeight
date: 2018-07-25 13:31:30
tags: [offsetParent,offsetTop,offsetHeight]
---

# offsetParent

**HTMLElement.offsetParent** 是一个只读属性，返回一个指向**最近的**包含该元素的**定位元素**。

- 如果没有定位的元素，则 `offsetParent` 为最近的 `table`, `table cell` 或根元素（标准模式下为 `html`；quirks 模式下为 `body`）
- 当元素的 `style.display` 设置为 "none" 时，`offsetParent` 返回 `null`。
- `offsetParent` 很有用，因为 [`offsetTop`](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement/offsetTop) 和 [`offsetLeft`](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement/offsetLeft) 都是相对于其内边距边界的

```javascript
parentObj = element.offsetParent;
//parentObj 是一个对象引用，当前元素相对于该对象偏移（offset）。
```

#### 兼容性:

- 在 Webkit 中，如果元素为隐藏的（该元素或其祖先元素的 `style.display` 为 "none"），或者该元素的 `style.position` 被设为 "fixed"，则该属性返回 `null`。
- 在 IE 9 中，如果该元素的 `style.position` 被设置为 "fixed"，则该属性返回 `null`。（`display:none` 无影响。）

<!--more-->

<br/>

# offsetTop

**HTMLElement.offsetTop** 为只读属性，它返回当前元素相对于其 [`offsetParent`](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement/offsetParent) 元素的顶部的距离。 

```javascript
var d = document.getElementById("div1");
var topPos = d.offsetTop;
 
if (topPos > 10) {
  // div1 距离它的 offsetParent 元素的顶部的距离大于 10 px
}
```

<br/>

# offsetHeight

**HTMLElement.offsetHeight** 是一个只读属性，它返回该元素的像素高度，高度包含该元素的垂直内边距和边框，且是**一个整数**。

- 通常，元素的offsetHeight是一种元素CSS高度的衡量标准，**包括元素的边框、内边距和元素的水平滚动条**（如果存在且渲染的话），不包含:before或:after等伪类元素的高度。
- 对于文档的body对象，它包括代替元素的**CSS高度线性总含量高**。浮动元素的向下延伸内容高度是被忽略的。

```markdown
这个属性值会被四舍五入为整数值，如果你需要一个浮点数值，请用 element.getBoundingClientRect().
```

<br/>

# clientHeight

这个属性是只读属性，对于没有定义CSS或者内联布局盒子的元素为0，否则，它是元素内部的高度(单位像素)，包含内边距，但**不包括水平滚动条、边框和外边距**。 

<br/>

# clientTop

一个元素上边框的宽度（以像素表示），当没有指定边框厚底时，一般为0。 

<br/>

![](offsetParent-offsetTop-offsetHeight\746339-20150915163721539-1441659862.jpg)