---
title: CSS-background-clip与background-origin
date: 2018-05-25 23:33:37
tags: [css,background-clip,background-origin]
---

# 一、CSS3新属性

在CSS2中，背景图片定义的位置是相对于其包含元素的填充(padding)的外部界限的，所有的溢出都会扩展到边框之下。CSS3引入了两个新的属性，可以对其进行更精细的控制。

- 第一个属性是：background-clip 

- 第二个属性是：background-origin



对于这两个新属性，其对应的属性值是相同的：**border-box, padding-box, content-box**。它们的最根本的区别就是：<font color=#A52A2A size=4 >**background-clip 是对背景图片的裁剪，background-origin是对背景图片设置起始点<**/font>。 <br/>

**对于background-clip, 其关键字是指将背景图片以border的尺寸、以padding的尺寸，以content的尺寸进行切割，其得到的结果是不完整的背景，也就是其中的一部分(原理与截图差不多)。而且有一点要注意，background-clip的切割是对这个容器背景的切割(包括图片与背景颜色)。**<br/> 

**对于background-origin，其关键字是指将背景图片放置到border范围内，padding范围内、content范围内，其得到的结果是完整的背景(原理与图片的缩放相似)。与background-clip不同的是，它只是单纯设置背景图片的边界，并不会对背景颜色造成影响。**<br/>

<!--more--> 

#### 下面就拿其中一个属性对比一下： 

``` css
/*Compare: background-clip:content-box
           background-origin:content-box*/

div {
    width: 400px;
    height: 400px;
    border: solid 10px;
    background-color: black;
    background-image: url(img.jpg);

    background-repeat: no-repeat;
    padding: 100px;
    background-clip: content-box;
}

.clip {
    background-clip: content-box;
}


.origin {
    background-origin: content-box;
}
```

**初始状态：** 

![初始状态](CSS-background-clip与background-origin\20160923112242962.png)

 

 

**background-clip : content-box**

![background-clip](CSS-background-clip与background-origin\20160923112324666.png)

 

 

**background-origin : content-box**

![background-origin](CSS-background-clip与background-origin\20160923112404307.png)

 