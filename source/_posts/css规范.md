---
title: css规范
date: 2018-08-24 12:31:03
tags: [css,规范]
---

# **数值与单位**

- 当属性值或颜色参数为 0 – 1 之间的数时，**省略小数点前的 0** 。
- 当长度**值为 0 时省略单位**。
- 十六进制的颜色属性值**使用小写和尽量简写**。

<!--more-->

<br/>

# **样式属性顺序**

单个样式规则下的属性在书写时，应按功能进行分组，并以 `Positioning Model` > `Box Model` > `Typographic` > `Visual` 的顺序书写，提高代码的可读性。

- **如果包含 content 属性，应放在最前面；**
- `Positioning Model` **布局方式、位置**，相关属性包括：`position / top / right / bottom / left / z-index / display / float / …`
- `Box Model` 盒模型，相关属性包括：`width / height / padding / margin / border / overflow / …`
- `Typographic` 文本排版，相关属性包括：`font / line-height / text-align / word-wrap / …`
- `Visual `视觉外观，相关属性包括：`color / background / list-style / transform / animation / transition / …`

注：

`Positioning` 处在第一位，因为他可以使一个元素脱离正常文本流，并且覆盖盒模型相关的样式。

盒模型紧跟其后，因为他决定了一个组件的大小和位置。

其他属性只在组件内部起作用或者不会对前面两种情况的结果产生影响，所以他们排在后面。

<br/>

# **尽量避免使用标签名**

假设我们有如下 html 结构：

```html
<div class="g-content">
    <ul class="g-content-list">
        <li class="item" />
        <li class="item" />
        <li class="item" />
        <li class="item" />
    </ul>
</div>
```



在给最里层的标签命名书写样式的时候，我们有两种选择：

```css
.g-content {
    .g-content-list {
        li {
            ...
        }
    }
}
```

或者是

```css
.g-content {
    .g-content-list {
        .item {
            ...
        }
    }
}
```

也就是，编译之后生成了下面这两个，到底使用哪个好呢？

- `.g-content .g-content-list li { }`
- `.g-content .g-content-list .item { }`

基于 CSS 选择器的解析规则（从右向左），建议使用上述第二种` .g-content .g-content-list .item { }` ，**避免使用通用标签名作为选择器的一环可以提高 CSS 匹配性能**。

> 浏览器的排版引擎解析 CSS 是基于从右向左（right-to-left）的规则，这么做是为了使样式规则能够更快地与渲染树上的节点匹配。

<br/>