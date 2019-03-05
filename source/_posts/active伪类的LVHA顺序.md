---
title: active伪类的LVHA顺序
date: 2019-03-05 18:27:37
tags: [LVHA,伪类]
---

#### 层叠样式表

`CSS(Cascading Style Sheets )层叠样式表`，所谓层叠就是**后面的样式会覆盖前面的样式**。所以在样式表中，各样式排列的顺序很有讲究。 

<br/>

#### `:active`  

`:active`伪类匹配被用户激活的元素，让页面能在浏览器监测到激活时给出反馈。

- 当用**鼠标交互**时，它代表的是用户**按下按键和松开按键之间**的时间。

- `:active` 伪类通常用来匹配**`tab键`交互**。


注：通常用于但并不限于 `a` 和 `button`HTML元素

<!--more-->

<br/>

#### LVHA顺序

`:active`伪类可能会被后声明的其他链接相关的伪类覆盖，这些伪类包括 [`:link`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:link)，[`:hover`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:hover)和 [`:visited`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:visited)。为了正常加上样式，需要把 `:active的样式放在所有链接相关的样式后，这种链接伪类先后顺序被称为`LVHA顺序：

```
:link —> :visited —> :hover —> :active
```

`:link`对指向**未被访问页面的链接**设置样式，**根据浏览器的浏览历史** 

`:visited`对指向**已被访问页面的链接**设置样式，**根据浏览器的浏览历史** 

注：`:link`和`:visited` 调换位置不影响样式

<br/>

#### `:visited ` 

出于隐私原因，使用此选择器修改的样式有较多[限制](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:visited#%E9%99%90%E5%88%B6) 

注：

- 未设置颜色或透明的属性不能使用`:visited` 

<br/>