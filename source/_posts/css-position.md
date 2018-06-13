---
title: css-position
date: 2018-06-12 19:14:41
tags: [css,position]
---

# 一、定位类型

- **相对定位**元素（relatively positioned element）是[计算后](https://developer.mozilla.org/zh-CN/docs/Web/CSS/computed_value)位置属性为 `relative `的元素。
- **绝对定位**元素（absolutely positioned element）是[计算后](https://developer.mozilla.org/zh-CN/docs/Web/CSS/computed_value)位置属性为 `absolute` 或 `fixed` 的元素。
- **粘性定位**元素（stickily positioned element）是[计算后](https://developer.mozilla.org/zh-CN/docs/Web/CSS/computed_value)位置属性为 `sticky` 的元素。

<br/>

大多数情况下，`height`和`width`被设定为auto的绝对定位元素，按其内容大小调整尺寸。但是，被<font color=#A52A2A size=4 >**绝对定位**</font>的元素可以通过：

- <font color=#A52A2A size=4 >**指定`top`和`bottom`，保留`height`未指定（即`auto`），来填充可用的垂直空间</font>**。
- <font color=#A52A2A size=4 >**指定`left`和 `right`，并将`width`指定为`auto`，来填充可用的水平空间**</font>。 

- 如果`top`和`bottom`都被指定（技术上，而不是`auto`），`top` 胜出。
- 如果指定了`left` 和`right`两侧，则在`direction`为ltr（英语，水平日语等）时`left`赢，并且在`direction`为rtl时`right`赢（阿拉伯文，希伯来文等）。

<br/>

# 二、取值

- `static`

  该关键字指定元素使用正常的布局行为，即元素在文档常规流中当前的布局位置。此时 **`top`, `right`, `bottom`, `left` 和 `z-index `属性无效**。



- `relative`

  该关键字下，元素先放置在未添加定位时的位置，再**在不改变页面布局的前提下调整元素位置**（因此会在此元素未添加定位时所在位置留下空白）。position:relative 对 table-*-group, table-row, table-column, table-cell, table-caption 元素无效。



- `absolute`

  **不为元素预留空间**，通过指定元素相对于最近的**非 static 定位祖先元素**的偏移，来确定元素位置。绝对定位的元素可以设置外边距（margins），且**不会与其他边距合并**。



- fixed

  **不为元素预留空间**，而是通过指定元素相对于**屏幕视口（viewport）**的位置来指定元素位置。元素的位置在屏幕滚动时不会改变。打印时，元素会出现在的每页的固定位置。`fixed`属性会创建新的层叠上下文。<font color=#A52A2A size=4 >**当元素祖先的 `transform`  属性非 `none` 时，容器由视口改为该祖先。**</font>



- `sticky` 

  **根据文档的正常流程进行定位**，然后根据`top`、`right`、`bottom`、 `left` ，相对于其**最近的滚动祖先**和[containing block](https://developer.mozilla.org/zh-CN/docs/Web/CSS/All_About_The_Containing_Block) （**最近的块级祖先**）偏移。偏移量不会影响任何其他元素的位置。

  该值始终创建一个新的堆叠上下文(stacking context)。请注意，粘性元素“粘”到最近的具有“**滚动机制**”的祖先（`overflow` 是 `hidden`, `scroll`, `auto`, 或 `overlay` ），即使该祖先不是最接近的实际滚动祖先。这有效地抑制了任何“粘性”行为 。

  - 当**最近的滚动祖先**和**最近的块级祖先**不是同一个元素时，**元素大小**根据**最近的块级祖先**计算，**元素位置**根据**最近的滚动祖先**计算。
  - 如果**最近的块级祖先**的高度**小于或者等于**元素的高度，**仍然有粘性效果**，但观察不出来。因为元素本身和**最近的块级祖先**<font color=#A52A2A size=4 >**同时进出**</font>**最近的滚动祖先**。

  <br/>

## 三、确定包含块

一个元素的**大小**和**位置**经常受其包含块的影响 （`width`、`height`、`top`等属性的百分比值是根据包含块大小计算的），确定其包含块的过程完全依赖于这个元素的 `position`属性： 

1. 如果 position 属性为 **`static`、  `relative`、`sticky`** ，其包含块就是由它的**最近的祖先块元素****（比如说inline-block, block 或 list-item元素）或**格式化上下文**(比如说 a table container, flex container, grid container, or the block container itself)的内容区的边缘组成的。

   

2. 如果 position 属性为 `absolute` ，包含块就是由它的最近的 position 的值不是 `static`（`fixed`, `absolute`, `relative` 或 `sticky`）的祖先元素的内边距区的边缘组成。

   

3. 如果 position 属性是 `fixed`，包含块就是由 [viewport](https://developer.mozilla.org/en-US/docs/Glossary/viewport) (in the case of continuous media) 组成的。

   

4. 如果 position 属性是` absolute`或 `fixed`，包含块也可能是由满足以下条件的最近父级元素的内边距区的边缘组成的：

   1. A `transform` or `perspective` value other than `none`
   2. A `will-change` value of `transform` or `perspective`
   3. A `filter` value other than `none` or a `will-change` value of `filter` (only works on Firefox).

