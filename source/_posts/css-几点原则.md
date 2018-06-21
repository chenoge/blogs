---
title: css-几点原则
date: 2018-03-10 19:47:22
tags: [css,原则]
---

# 1.不要写不需要的样式定义

- 减少CSS文件的长度，以便浏览。
- 明确你的CSS类需要做什么，而不是定义一堆已经产生的垃圾。

<br/>

# 2.将CSS看作可重用组件

定义可重用的CSS和组件以供自己使用，则可以减少很多复杂性，重用类的作用：

- 确保你的设计风格在不同的页面之间保持一致
- 提高编写CSS效率

```css
.hide {
    display: none;
}

.text-center {
    text-align: center;
}

.padding-0 {
    padding: 0;
}

.padding-xxs {
    padding: 5px;
}

.padding-xs {
    padding: 10px;
}

.padding-sm {
    padding: 20px;
}

.padding-md {
    padding: 30px;
}

.padding-lg {
    padding: 40px;
}

.padding-xl {
    padding: 50px;
}

.padding-xxl {
    padding: 60px;
}
```

<br/>

# 3.除非绝对需要，否则避免嵌套

```css
a {
    color: blue;
    &:hover {
        color: black;
    }
}

.link--red {
    color: red;
}
```

<br/>

# 4.利用BEM防止嵌套

防止过度嵌套的一个策略是名为[BEM（Block Element Modifier）](https://link.zhihu.com/?target=https%3A//csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)的命名策略 

BEM的意思就是块（block）、元素（element）、修饰符（modifier），是由Yandex团队提出的一种CSS Class 命名方法。 

类似于：

```css
.block{}	
.block__element{}		
.block--modifier{}
.block__element--modifie{}	

/**或者*/

.type-block{}
.type-block__elementr{}
.type-block_modifier{}
.type-block__element_modifier{}
```

- block：封装一个**独立的实体**，它本身是有意义的。尽管块可以嵌套并相互交互，但在语义上它们保持平等; 没有优先级或层次结构 。
- 块的一部分，**没有独立的含义**。任何元素都在语义上与其块相关联。 
- 在块或元素上的标志。使用它们来改变**外观，行为或状态**。 

<br/>

```html
<div class=”profile”>
    <img src=”person.jpg” class=”profile__photo”/>
</div>
```

```css
.profile {
    background-color: white;
    border: 1px solid #ccc;
}

.profile__photo {
    border-radius: 50%;
    border: 1px solid #000;
}
```

<br/>

# 5.只使用 inportant 作为最后的手段

