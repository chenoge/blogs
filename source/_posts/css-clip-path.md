---
title: css-clip-path
date: 2018-06-15 15:46:19
tags: [css,clip-path]
---

# 一、clip-path 

```css
.clip-me {
    /* 剪裁路径来自内联SVG <clipPath>元素 */
    clip-path: url(#c1); 
    
    
    /* 路径来自外部SVG */
    clip-path: url(path.svg#c1);
    
    
    /*带圆角的矩形*/
    /*前四个参数代表上、右、下、左到边框的距离*/
    /*后四个参数代表上、右、下、左的圆角半径大小，跟border-radius类似*/
    clip-path: inset(10% 20px 30px 5% round 14px 50% 10% 50%);
    clip-path: inset(10% 20px 30px 5% round 14px 50% 10% 50%/10px 50% 2% 50%);

    
    /* 多边形 */
    clip-path: polygon(5% 5%, 100% 0%, 100% 75%, 75% 75%, 75% 100%, 50% 75%, 0% 75%);
    
    
    /* 圆 */
    clip-path: circle(30px at 35px 35px);
    
    
    /* 椭圆 */
    clip-path: ellipse(65px 30px at 125px 40px);
}
```

