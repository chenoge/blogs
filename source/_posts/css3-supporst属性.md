---
title: supporst属性
date: 2018-12-07 10:53:22
tags: [css,supporst]
---

`@supports`是`CSS3`新引入的规则之一，主要用于检测当前浏览器是否支持某个`CSS`属性并加载具体样式。

#### 1、基本使用

```css
@supports (display: grid) {
    .container {
        color: red;
    }
}
```

类似`@media`媒体查询，当浏览器支持`display:grid`这个CSS属性时才应用其中的样式。

<br/>

<!--more-->

#### 2、逻辑运算

```css
/**not*/
@supports not(display: grid){...}

 /**and*/
@supports (display: grid) and (position: sticky){...}

/**or*/
@supports (display: grid) or (display: flex){...}
```

<br/>

注：括号内不一定都要是“关键字”，只要是CSS语法都可以 

```css
@supports (border-radius: 4px) or (--btn-color: red){...}
```

<br/>

#### 3、js方法查询

```javascript
if(CSS.supports('display', 'grid')){
    alert('it support!');
}
```

