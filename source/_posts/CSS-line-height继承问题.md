---
title: line-height继承问题
date: 2018-05-27 23:21:48
tags: [css,line-height]
---

#### 一、line-height值

| normal  | 默认。设置合理的行间距。                             |
| ------- | ---------------------------------------------------- |
| number  | 设置数字，此数字会与当前的字体尺寸相乘来设置行间距。 |
| length  | 设置固定的行间距。                                   |
| %       | 基于当前字体尺寸的百分比行间距。                     |
| inherit | 规定应该从父元素继承 `line-height` 属性的值。        |

注：`normal`我们常常认为它是（或者应该是）`1`或者`1.2`，甚至也可以说，[CSS规范](https://www.w3.org/TR/CSS2/visudet.html#propdef-line-height)都不清楚是哪一个。

```
normal: Tells user agents to set the used value to a "reasonable" value based on the font of the element. The value has the same meaning as <number>. We recommend a used value for 'normal' between 1.0 to 1.2. The computed value is 'normal'.
```

<br/>

### 二、继承问题

- 如果父级的`line-height`属性值**有单位或百分比**，那么**子级继承的值则是换算后的一个具体的px级别的值**； 
- 而如果父级的`line-height`属性值**没有单位**，则**子级会直接继承这个“数值”**，而非计算后的具体值，此时子级的`line-height`会根据本身的`font-size`值重新计算得到新的`line-height`值。

<br/>

### 三、font-size 与line-height

每个元素使用相同的`font-size`，但使用不同的`font-family`，但渲染出来的`line-height`是不同的。

CSS 权威指南`基本视觉格式化`一章中讲到：对于行内非替换元素或者匿名文本来说， `font-size` 指定了它们的 `content area `的高度，由于` inline box` 是由 `content area` 加上`上下`的 `half-leading `构成的，那么如果元素的` leading `为 0，在这种情况下，`font-size` 指定了` inline box` 的高度。

<br/>

### 四、[leading](http://www.ituring.com.cn/article/18076) 

1、英文字体有**基线**（`baseline`）和**中线**（`meanline`），这两条线之间就是所谓的`x-height`，即`小写字母x`的高度。基线之上的部分是**上伸区域**（`ascent`），基线之下的部分是**下伸区域**（`descent`）。



2、两种说法

![](CSS-line-height继承问题\01RTdJhLw8R4.png)

- **现代排版软件** ：两行文本的基线之间的距离是**现代排版软件**中所说的行距（leading） 

<br/>

- **CSS** ： `leading = line-height - font-size` 

![](CSS-line-height继承问题\01RTdOdi3s7A.png)

<br/>

