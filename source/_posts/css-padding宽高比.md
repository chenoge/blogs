---
title: css-padding宽高比
date: 2017-09-24 22:17:19
tags: [css,padding]
---

对于padding属性而言，任意方向的百分比padding都相对于宽度计算可以让我们轻松实现固定比例的块级容器。 

```css
div { padding: 50%; }
div { padding: 100% 0 0; }
div { padding-bottom: 100%; }


.banner {
    padding: 15.15% 0 0;
    position: relative;
}
	
.banner > img {
    position: absolute;
    width: 100%; 
    height: 100%;
    left: 0; top: 0;
}
```

注：

1、图片宽度是100%容器的，padding的15.5%其实就是图片的高宽比例。

2、图片宽度50%容器宽度，图片高宽比4:3，我们也可以这么写

```css
.img-box {
    padding: 0 50% 66.66% 0;
}

```

3、如果没有text-align属性干扰，img的position，及left和top可以不用指定 