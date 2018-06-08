---
title: flex瀑布流布局
date: 2018-05-30 15:42:05
tags: [css,flex,瀑布流]
---



``` html
<div class="masonry">
    <div class="item">
        <div class="item__content">
        </div>
    </div>
    <div class="item">
        <div class="item__content item__content--small">
        </div>
    </div>
</div>
```

``` scss
.masonry {
    display: flex;
    flex-flow: column wrap;
    width: 100%;
    height: auto;
    
    /*缺点：需要设置一个固定的height*/
    @media screen and (min-width: 400px) {
        height: 1600px;
    }

    @media screen and (min-width: 600px) {
        height: 1300px;
    }

    @media screen and (min-width: 800px) {
        height: 1100px;
    }

    @media screen and (min-width: 1100px) {
        height: 800px;
    }
}
```
