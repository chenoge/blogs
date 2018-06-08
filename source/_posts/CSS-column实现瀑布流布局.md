---
title: column实现瀑布流布局
date: 2018-05-30 15:42:20
tags: [css,column，瀑布流]
---

# 一、属性

- **column-count**:      最理想的分栏数目
- **column-gap**:      栏目之间的水平间隙
- **column-rule**:      分割线，形式规则什么的等同于border
- <font color=#A52A2A size=4 >**break-inside**:      **内容盒子如何中断**</font>

``` css
/*瀑布流层*/

.waterfall {
    column-count: 4;
    column-gap: 1em;
}


/*一个内容层*/

.item {
    padding: 1em;
    margin: 0 0 1em 0;
    break-inside: avoid;
    border: 1px solid #000;
}


.item img {
    width: 100%;
    margin-bottom: 10px;
}
```


``` html
<div class="waterfall">
    <div class="item">
        <img src="https://imgsa.jpg">
        <p>1 convallis timestamp</p>
    </div>
</div>
```

