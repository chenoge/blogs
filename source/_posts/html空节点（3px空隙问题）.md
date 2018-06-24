---
title: html空节点（3px空隙问题）
date: 2017-07-31 20:32:45
tags: [3px,空隙,空节点]
---

# 换行与空隙

`html换行`被解析为**空格**。也是常说的**3像素空隙**的问题，根据测试不同浏览器产生的空隙大小会不一样。

Chrome，Firefox，IE8+都存在这样的问题，**浏览器把换行或空格解析成了“空白节点”**，就是javascript中`nodeType`等于3的节点，IE6,7是忽略这个节点的。 

<!--more-->

```css
span {
    border: 1px solid red;
    padding: .5em 1em;
}
```

```html
<span>1</span>
<span>2</span>
<span>3</span>
```

<br/>

# 处理方法

#### 1、不换行

```html
<span>1</span><span>2</span><span>3</span>
```

<br/>

#### 2、设置margin-left为负值 

```css
span {
    border: 1px solid red;
    padding: .5em 1em;
    margin-left: -3px;
}
```

<br/>

#### 3、设置父元素字体大小为0 

```css
div {
    font-size: 0px;
}

span {
    border: 1px solid red;
    padding: .5em 1em;
    font-size: 12px;
}
```

```html
<div>
    <span>1</span>
    <span>2</span>
    <span>3</span>
</div>
```

