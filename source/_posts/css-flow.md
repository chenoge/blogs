---
title: css-flow
date: 2018-06-12 18:53:43
tags: [css,flow]
---

## in-flow和out-of-flow

### [规范规定](http://www.w3.org/TR/CSS2/visuren.html#positioning-scheme)

```markdown
An element is called out of flow if it is floated, absolutely positioned, or is the root element. An element is called in-flow if it is not out-of-flow.
```

如果一个元素是**浮动**的(float:left/right)，**绝对定位**的(position:absolute/fixed)或者是**根元素**(html)，那么它被称之为**流外的元素**(out-of-flow)。如果一个元素不是流外的元素，那么它被称之为**流内的元素**(in-flow)。

```markdown
The flow of an element A is the set consisting of A and all in-flow elements whose nearest out-of-flow ancestor is A.
```

元素 A 的流是一个集合，包含 A 元素本身，以及元素 A 的流内的子元素且这些子元素最近的流外祖先是 A 元素。

### 示例

```html
<div class="A" style="position: absolute;">
  <div class="B"></div>
  <div class="C" style="position:absolute">
    <div class="D"></div>
  </div>
  <div class="E">
  <div class="F"></div>
</div>
```

以上示例中，A 元素的流包含分析如下：

1. A 和 C 是流外的元素，所以 C 被排除
2. D 元素由于最近的流外祖先是 C，所以他也不是 A 的流
3. 所以最终 A 元素流内的元素只剩下：ABEF