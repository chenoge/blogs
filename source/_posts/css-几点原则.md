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

