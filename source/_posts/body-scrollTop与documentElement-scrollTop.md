---
title: document.body.scrollTop与document.documentElement.scrollTop
date: 2018-10-13 13:34:10
tags: [document.body.scrollTop,document.documentElement.scrollTop]
---

#### 古代浏览器间的 scrollTop差异

- **IE6/7/8：**使用 `document.documentElement.scrollTop`； 
- **IE9及以上：**使用`window.pageYOffset`或者`document.documentElement.scrollTop `
- **Safari:**  `window.pageYOffset` 与document.body.scrollTop都可以； 
- **Firefox:** window.pageYOffset 或者 `document.documentElement.scrollTop `；
- **Chrome：**只认识`document.body.scrollTop`;

注：document.body.scrollTop与document.documentElement.scrollTop同时只会有一个值生效。

```javascript
let sTop = document.body.scrollTop || document.documentElement.scrollTop;
```



- ##### 2018-10-30更新：**浏览器 =》古代浏览器**

  **chrome:**

  -  html元素作为body元素的父类，遵行普通父元素与子元素之间的关系
  -  `document.body`作为body元素`dom对象`的引用
  -  `document.documentElement`作为html元素`dom对象`的引用



<br/>

#### document.compatMode

```
mode = document.compatMode
```

- 如果文档处于“怪异模式”，则该属性值为`"BackCompat"`
- 如果文档处于“标准模式”或者“准标准模式(almost standards mode)”，则该属性为`"CSS1Compat"`

注：还有另外一种渲染模式, [Gecko的"准标准模式"](https://developer.mozilla.org/zh-cn/Gecko's_%22Almost_Standards%22_Mode), 该模式和标准规范模式的区别仅为表格单元内的图片布局方式不同. 且该模式的类型字符串仍为: "CSS1Compat". 

<br/>

#### Document.scrollingElement

**scrollingElement** （ [`Document`](https://developer.mozilla.org/zh-CN/docs/Web/API/Document) 的只读属性）返回滚动文档的 [`Element`](https://developer.mozilla.org/zh-CN/docs/Web/API/Element) 对象的引用。 

在标准模式（`Standards Mode`）下, 这是文档的根元素, [`document.documentElement`](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/documentElement).

当在怪异模式（`Quirks Mode `）下， `scrollingElement` 属性返回 HTML `body` 元素（若不存在返回 null ）。

<br/>

#### Window.scrollTo

```javascript
// window.scrollTo(x-coord,y-coord )
window.scrollTo( 0, 1000 );

// window.scrollTo(options)
// 设置滚动行为改为平滑的滚动
window.scrollTo({ 
    top: 1000, 
    behavior: "smooth" 
});
```

- `x-coord` 是文档中的横轴坐标。
- `y-coord` 是文档中的纵轴坐标。
- `options` 是一个包含三个属性的对象:
  - `top` 等同于  `y-coord`
  - `left` 等同于  `x-coord`
  - `behavior`  类型 String，表示滚动行为。
    - 支持参数 smooth(平滑滚动)，instant(瞬间滚动)
    - 默认值auto，实测效果等同于instant

**注：window.scrollTo滚动的容器为`document.documentElement`**

<br/>

#### window.scrollBy

```javascript
window.scrollBy(x-coord, y-coord);
window.scrollBy(options)
```

注：`window.scrollBy`的参数与`window.scrollTo`一样，但是`window.scrollBy`中的`x-coord` 、`y-coord`、`top`、`left` 可以是**负值**。`window.scrollBy`是**以自身为参考点**，`window.scrollTo`**以浏览器窗口为参考点**。

<br/>

