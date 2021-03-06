---
title: white-space与换行
date: 2018-12-14 23:16:25
tags: [white-space]
---

`white-space` CSS 属性是用来设置如何处理元素中的空白。

```
white-space: normal | pre | nowrap | pre-wrap | pre-line
```
`normal`：连续的空白符会被合并，换行符会被当作空白符来处理。填充line盒子时，必要的话会换行。

`nowrap`：和 `normal` 一样，连续的空白符会被合并。但文本内的换行无效。

`pre`：连续的空白符会被保留。在遇到换行符或者` <br/>`元素时才会换行。 

`pre-wrap`：连续的空白符会被保留。在遇到换行符或者` <br/>`元素，或者需要为了填充line盒子时才会换行。

`pre-line`：连续的空白符会被合并。在遇到换行符或者` <br/>`元素，或者需要为了填充line盒子时会换行。

<br/>

<!--more-->

|            | 换行符 | 空格和制表符 | 文本超出容器宽度 |
| ---------- | ------ | ------------ | ---------------- |
| `normal`   | 合并   | 合并         | 换行             |
| `nowrap`   | 合并   | 合并         | 不换行           |
| `pre`      | 保留   | 保留         | 不换行           |
| `pre-wrap` | 保留   | 保留         | 换行             |
| `pre-line` | 保留   | 合并         | 换行             |