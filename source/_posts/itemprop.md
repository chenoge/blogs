---
title: itemprop
date: 2018-11-19 22:45:16
tags: [itemprop]
---

### 一、`itemprop`属性

全局属性 `itemprop`被用于向一个物体中添加属性。每一个HTML元素都可以指定一个itemprop属性，一个`itemprop`属性由name-value对组成。属性值可以是一个string或者一个URL，并且可以和大部分元素进行组合，包括`img`、`video`、`link`、`source` 、`audio `等。

<!--more-->

<br/>

### 二、结构化数据 

```html
<div itemscope itemtype ="http://schema.org/Movie">
  <h1 itemprop="name">Avatar</h1>
  <span>Director:
    <span itemprop="director">James Cameron</span>
    (born August 16, 1954)</span>
  <span itemprop="genre">Science fiction</span>
  <a href="../movies/avatar-theatrical-trailer.html" itemprop="trailer">Trailer</a>
</div>
```

<br/>

### 三、`itemscope` 

`itemscope` 是一个布尔值的 全局属性。它定义了一个与元数据关联的数据项。就是说一个元素的 `itemscope` 属性会创建一个项，包含了一组与元素相关的键值对。 相关的属性 `itemtype`通常表示表中一个有效的 URL （比如 [schema.org](http://schema.org/)） 来表述项目和上下文。下面每个例子中的概念表都来自 [schema.org](https://schema.org/). 



每个 HTML 元素都可以有指定的 `itemscope` 属性。一个具有 `itemscope` 属性的元素可以没有关联的 `itemtype` ，但必须有相关的 `itemref`。 



Schema.org 提供了一份共享的词汇表，站长可以使用它来标记网页，而这些标记则被主要的搜索引擎： Google， Microsoft， Yandex 和 Yahoo! 所支持。 