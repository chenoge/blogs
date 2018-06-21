---
title: height-100%问题
date: 2018-01-04 23:02:43
tags: [height,100%]
---

# 一、height:auto

- 因为页面没有缺省的高度值，当设置百分百的高度时，无法根据父元素获取高度 

- 父元素的高度只是一个缺省值height：auto，只有子元素撑开父元素

只要给父元素设置一个缺省值 

```css
html,body{  
    margin: 0;  
    padding: 0;  
    height: 100%;  
    width: 100%;  
}
```

<br/>

# 二、line-height和height

CSS中对高度起作用的是`height`和`line-height`。如果一个标签没有定义`height`属性(包括百分比高度)，那么其最终表现的高度一定是由`line-height`起作用 。

<br/>

在`inline box`模型中，有个`line boxes`，这玩意是看不见的，这个玩意的工作就是包裹每行文字。**一行文字一个`line boxes`**。`line boxes`什么特性也没有，就高度。所以**一个没有设置`height`属性的`div`的高度就是由一个一个`line boxes`的高度堆积而成的。**

<br/>

其实`line boxes`不是直接的生产者，属于中层干部，真正的活儿都是它的手下 – `inline boxes`干的。`line boxes`只是个考察汇报人员，考察它的手下谁的实际`line-height`值最高，谁最高，它就要谁的值，然后向上汇报，形成高度。

<br/>

# 三、垂直居中

- `line-height`值设置为`height`一样大小的值可以实现单行文字的垂直居中 (**`height`是多余的**)
- 把`line-height`设置为您需要的`box`的大小可以实现单行文字的垂直居中 