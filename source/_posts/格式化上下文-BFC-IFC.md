---
title: 格式化上下文-BFC-IFC
date: 2018-03-17 18:15:25
tags: [BFC,IFC]
---

想要理解**BFC**与**IFC**，首先要理解另外两个概念：**Box** 和 **FC**（即 formatting context） 

<br/>

# 一、Box

一个页面是由很多个 Box 组成的，元素的类型和 display 属性决定了这个 Box 的类型。不同类型的 Box，会参与不同的 Formatting Context。 

- `Block level`的box会参与形成**BFC**，比如display值为`block`，`list-item`，`table`的元素。
- `Inline level`的box会参与形成**IFC**，比如display值为`inline`，`inline-table`，`inline-block`的元素。

<br/>

# 二、FC（Formatting Context）

它是W3C CSS2.1规范中的一个概念，定义的是页面中的一块渲染区域，并且有一套渲染规则，它决定了其子元素将如何定位，以及和其他元素的关系和相互作用。 常见的Formatting Context 有：

- Block Formatting Context（BFC | 块级格式化上下文）
- Inline Formatting Context（IFC |行内格式化上下文）

<br/>

# 三、IFC布局规则

- 在**行内格式化上下文**中，框(boxes)一个接一个地**水平排列**，**起点是`包含块`的顶部**
- 水平方向上的 `margin`，`border` 和 `padding`在框之间得到**保留**
- 框在垂直方向上可以以**不同的方式对齐**：它们的顶部或底部对齐，或根据其中文字的基线对齐
- <font color=#A52A2A size=4 >包含那些框的**长方形区域**，会形成一行，叫做`行框` </font>

<br/>

# 四、BFC布局规则

- **内部的Box**会一个接一个地**垂直放置**
- 每个元素的左外边缘（margin-left)， 与包含块的左边（contain box left）相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。除非这个元素自己形成了一个新的BFC。
- **BFC的区域不会与float box`重叠`**。
- BFC就是页面上的一个隔离的**独立容器**，容器里面的子元素**不会影响到外面的元素**。反之也如此。
- **计算BFC的高度时，浮动子元素也参与计算**

<br/>

# 五、形成一个BFC

下列方式会创建**块格式化上下文**：

- **根元素**或**包含根元素的元素**
- **浮动元素**（元素的 [`float`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/float) 不是 `none`）
- **绝对定位元素**（元素的 [`position`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/position) 为 `absolute` 或 `fixed`）
- **行内块元素**（元素的 [`display`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/display) 为 `inline-block`）
- **表格单元格**（元素的 [`display`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/display)为 `table-cell`，HTML表格单元格默认为该值）
- **表格标题**（元素的 [`display`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/display) 为 `table-caption`，HTML表格标题默认为该值）
- 匿名表格单元格元素（元素的 [`display`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/display)为 `table、``table-row`、 `table-row-group、``table-header-group、``table-footer-group`（分别是HTML table、row、tbody、thead、tfoot的默认属性）或 `inline-table`）
- [`overflow`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/overflow) 值不为 `visible` 的块元素
- [`display`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/display) 值为 `flow-root` 的元素
- [`contain`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/contain) 值为 `layout`、`content`或 `strict` 的元素
- **弹性元素**（[`display`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/display)为 `flex` 或 `inline-flex`元素的**直接子元**素）
- **网格元素**（[`display`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/display)为 `grid` 或 `inline-grid` 元素的**直接子元素**）
- **多列容器**（元素的 [`column-count`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/column-count) 或 [`column-width`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/column-width) 不为 `auto，包括 ``column-count` 为 `1`）
- `column-span` 为 `all` 的元素始终会创建一个新的BFC，即使该元素没有包裹在一个多列容器中（[标准变更](https://github.com/w3c/csswg-drafts/commit/a8634b96900279916bd6c505fda88dda71d8ec51)，[Chrome bug](https://bugs.chromium.org/p/chromium/issues/detail?id=709362)）

<br/>

# 六、外边距合并

**块级元素**的[上外边距](https://developer.mozilla.org/zh-CN/docs/Web/CSS/margin-top)和[下外边距](https://developer.mozilla.org/zh-CN/docs/Web/CSS/margin-bottom)有时会合并（或折叠）为一个外边距，其大小取其中的最大者，这种行为称为**外边距折叠**（margin collapsing） 

注：[浮动元素](https://developer.mozilla.org/zh-CN/docs/Web/CSS/float)和[绝对定位元素](https://developer.mozilla.org/zh-CN/docs/Web/CSS/position#absolute)的外边距不会折叠（触发了 [块格式化上下文 Block Formatting Context， BFC](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Block_formatting_context)） 

<br/>

下面列出了会发生外边距折叠的三种基本情况：

- 相邻元素之间

  毗邻的两个元素之间的外边距会折叠（除非后一个元素需要[清除之前的浮动](https://developer.mozilla.org/zh-CN/docs/Web/CSS/clear)）。



- **父元素**与其**第一个**或**最后一个**子元素之间

  如果在父元素与其第一个子元素之间不存在**边框、内边距、行内内容**，也没有创建[块格式化上下文](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Block_formatting_context)、或者[清除浮动](https://developer.mozilla.org/zh-CN/docs/Web/CSS/clear)将两者的 [`margin-top`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/margin-top) 分开；

  或者在父元素与其最后一个子元素之间不存在**边框、内边距、行内内容**、[`height`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/height)、[`min-height`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/min-height)、[`max-height`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/max-height)将两者的 [`margin-bottom`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/margin-bottom) 分开，那么这两对外边距之间会产生折叠。此时子元素的外边距会“溢出”到父元素的外面。



- 空的块级元素

  如果一个块级元素中不包含任何内容，并且在其 [`margin-top`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/margin-top) 与 [`margin-bottom`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/margin-bottom) 之间没有**边框、内边距、行内内容**、[`height`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/height)、[`min-height`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/min-height) 将两者分开，则该元素的上下外边距会折叠。

<br/>

# 七、BFC用处

#### 1. 清除浮动

```html
<div class="wrap">
    <section>1</section>
    <section>2</section>
</div>
```



```css
.wrap {
    border: 2px solid yellow;
    width: 250px;
}

section {
    background-color: pink;
    float: left;
    width: 100px;
    height: 100px;
}
```

可以看到，由于子元素都是浮动的，受浮动影响，边框为黄色的父元素的高度塌陷了。 

![](格式化上下文-BFC-IFC\1.png)

解决方案：为 `.wrap` 加上 `overflow: hidden`;使其形成`BFC`，根据BFC规则第六条，计算高度时就会计算float的元素的高度，达到清除浮动影响的效果。 

![](格式化上下文-BFC-IFC\2.png)



#### 2. 防止垂直margin合并

```html
<section class="top">1</section>
<section class="bottom">2</section>
```

```css
section {
    background-color: pink;
    margin-bottom: 100px;
    width: 100px;
    height: 100px;
}

.bottom {
    margin-top: 100px;
}
```

![](格式化上下文-BFC-IFC\3.png)

可以看到，明明.top和.bottom中间加起来有200px的margin值，但是我们只能看到100px。这是因为他们的外边距相遇发生了合并。

 

怎样解决：为其中一个元素的外面包裹一层元素。并为这个外层元素设置 overflow: hidden;，使其形成BFC。因为BFC内部是一个独立的容器，所以不会与外部相互影响，可以防止margin合并。

![](格式化上下文-BFC-IFC\4.png)

```html
<section class="top">1</section>
<div class="wrap">
    <section class="bottom">2</section>
</div>
```

```css
.wrap {
    overflow: hidden;
}
```



#### 2. 布局：自适应两栏布局

```html
<div>
    <aside></aside>
    <main>我是好多好多文字会换行的那种蛤蛤蛤蛤蛤蛤蛤蛤蛤蛤蛤蛤蛤</main>
</div>
```

```css
div {
    width: 200px;
}

aside {
    background-color: yellow;
    float: left;
    width: 100px;
    height: 50px;
}

main {
    background-color: pink;
}
```

可以看到右侧元素的一部分跑到了左侧元素下方

![](格式化上下文-BFC-IFC\5.png)

注：

- float元素的特点：**背景重叠**，**文字环绕**

- float元素和main发生了重叠，如果float元素背景色为透明，可以看到main的背景色。

<br/>

解决方案：为main设置 overflow: hidden; 触发main元素的BFC，根据规则第4、5条，BFC的区域是独立的，不会与页面其他元素相互影响，且不会与float元素`重叠`，因此就可以形成两列自适应布局 

![](格式化上下文-BFC-IFC\6.png)

