---
title: vertical-align与line-height
date: 2018-11-07 20:37:41
tags: [vertical-align,line-height]
---

# vertical-align

[CSS](https://developer.mozilla.org/en-US/docs/CSS) 的属性 **vertical-align** 用来指定行内元素（inline）或表格单元格（table-cell）元素的垂直对齐方式。 

<br/>

#### 相对父元素的值

这些值使元素相对其父元素垂直对齐：

- `baseline`

  使元素的**基线**与父元素的**基线**对齐。HTML规范没有详细说明部分[可替换元素](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Replaced_element)的基线，如[`textarea`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/textarea) ，这意味着这些元素使用此值的表现因浏览器而异。

  

- `sub`

  使元素的**基线**与父元素的**下标基线**对齐。

  

- `super`

  使元素的**基线**与父元素的**上标基线**对齐。

  

- `text-top`

  使元素的**顶部**与父元素的**字体顶部**对齐。

  

- `text-bottom`

  使元素的**底部**与父元素的**字体底部**对齐。

  

- `middle`

  使元素的**中部**与父元素的**基线加上父元素x-height**（译注：[x高度](https://www.zhangxinxu.com/wordpress/2015/06/about-letter-x-of-css/)）的一半对齐。

  

- [`height`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/length)

  使元素的**基线**对齐到父元素的**基线之上的给定长度**。可以是负数。

  

- [`percentage`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/percentage)

  使元素的**基线**对齐到父元素的**基线之上的给定百分比**，该百分比是[`line-height`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/line-height)属性的百分比。可以是负数。

  <br/>

  <!--more-->

#### 相对行的值

下列值使元素相对整行垂直对齐：

- `top`

  使元素及其后代元素的**顶部**与整行的**顶部**对齐。

  

- `bottom`

  使元素及其后代元素的**底部**与整行的**底部**对齐。

  

注：特别重要一点，**没有基线的元素，使用外边距的下边缘替代**，解析如下

<br/>

#### **`inline-block`和`baseline`** 

```markdown
The baseline of an ‘inline-block’ is the baseline of its last line box in the normal flow, unless it has either no in-flow line boxes or if its ‘overflow’ property has a computed value other than ‘visible’, in which case the baseline is the bottom margin edge.
```

**一个`inline-block`元素，如果里面没有`inline`内联元素，或者`overflow`不是`visible`，则该元素的`基线`就是其margin底边缘，否则，其基线就是元素里面`最后一行内联元素`的基线。** 

<br/>

# line-height

**行内框**，每个行内元素会生成一个行内框，行内框是一个浏览器渲染模型中的一个概念，无法显示出来，在没有其他因素影响的时候（padding等），行内框等于内容区域，而设定行高时行内框高度不变，半行距【（行高-字体size）/2】分别增加/减少到内容区域的上下两边（深蓝色区域）



**行框（line box），**行框是指本行的一个虚拟的矩形框，是浏览器渲染模式中的一个概念，并没有实际显示。行框高度等于本行内所有元素中行内框最大的值（**以行高值最大的行内框为基准，其他行内框采用自己的对齐方式向基准对齐，最终计算行框的高度**），当有多行内容时，每行都会有自己的行框。



```html
<div style="background-color:#ccc;">
    <span style="font-size:1em;background-color:#666;">中文English</span>
    <span style="font-size:3em;background-color:#999;">中文English</span>
    <span style="font-size:3em;background-color:#999;">English中文</span>
    <span style="font-size:1em;background-color:#666;">English中文</span>
</div>
```

![](vertical-align与line-height\snipaste20181107_212101.png)

