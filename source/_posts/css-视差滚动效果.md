---
title: css-视差滚动效果
date: 2018-03-19 19:30:32
tags: [css,视差]
---

```css
.demo {
    perspective: 2px; 
    padding: 0; 
    height: 600px; 
    height: calc(100vh - 300px); 
    overflow: auto;
}

.box {
    height: 1280px;
    transform-style: preserve-3d;
    position: relative;
}

.iphone {
    position: absolute; left: 50%;
    transform: translate3D(-50%, -120px, -4px) scale(3);
}
	
	
.smile, .flower, .music, .pdf { 
    /* 略... */ 
}
```

```html
<div class="demo">
    <div class="box">
        <img src="mobile_1_iphone.jpg" class="iphone">
        <i class="smile"></i>
        <i class="flower"></i>
        <i class="music"></i>
        <i class="pdf"></i>
    </div>
</div>
```

# 3D视角示意图

![](css-视差滚动效果\css3d-edited.png)

当我们在屏幕前面**2个单位(perspective: 2px; )**的地方，看屏幕后面**4个单位(transform: translate3D(-50%, -120px, -4px) scale(3);)**的元素，肉眼所见的画面大小只有**实际的1/3**，即所谓的近大远小。此时**scale(3)**让内容放大到原来3倍，正好在平面上看上去好像是原来大小。

<br/>

虽然肉眼所见体积似乎是1:1，但是，滚动时候的位移变化还是1:3。网页中的3D就是模拟真实世界的3D效果，因此，也会有这种视差体验。