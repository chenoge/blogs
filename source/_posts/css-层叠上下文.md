---
title: css-层叠上下文
date: 2018-01-21 12:30:45
tags: [css,层叠上下文,stacking-context]
---

# 一、定义

**层叠上下文**是HTML元素的三维概念，这些HTML元素在z轴上延伸。HTML元素依据其自身属性按照**优先级顺序**占用**层叠上下文的空间**。层叠上下文是可以相互嵌套的，一个层叠上下文中包含了普通元素以及子层叠上下文。

<br/>

# 二、形成

文档中的层叠上下文由满足以下任意一个条件的元素形成：

- **根元素** (HTML)
- z-index 值不为 "auto"的 **绝对/相对**定位
- 一个 z-index 值不为 "auto"的 flex 项目 (flex item)，即：父元素 display: flex|inline-flex
- opacity 属性值小于      1 的元素
- transform 属性值不为      "none"的元素
- mix-blend-mode 属性值不为 "normal"的元素
- filter值不为“none”的元素
- perspective值不为“none”的元素
- isolation 属性被设置为 "isolate"的元素
- position:      fixed
- 在 will-change 中指定了任意 CSS 属性，即便你没有直接指定这些属性的值
- -webkit-overflow-scrolling 属性被设置 "touch"的元素

注：

- 在层叠上下文中，其子元素同样也按照上面解释的规则进行层叠
- **其子元素的 z-index 值只在父级层叠上下文中有意义**
- 子级层叠上下文被自动视为**父级层叠上下文**的一个独立单元

- **没有创建自己的层叠上下文的元素 将被父层叠上下文包含**

<br/>

<!--more--> 

# 三、层叠顺序

一个层叠上下文中，元素发生层叠时有着特定的垂直显示顺序，这种顺序按照一定的规则生成

![](css-层叠上下文\2016-01-09_211116.png)

<br/>

# 四、层叠准则

- 谁大谁上：当具有明显层叠水平标示的时候，如z-index值，在同一层叠上下文领域，层叠水平值大的覆盖小的那个；（**层叠水平小的先被绘制**）

- 后来居上：当元素水平一致、**层叠顺序相同时**，在DOM流中处于后面的元素覆盖前面的元素。

<br/>

# 五、例子

- Root
  - DIV #1
  - DIV #2
  - DIV #3
    - DIV #4
    - DIV #5
    - DIV #6



![](css-层叠上下文\=Understanding_zindex_04.png)



