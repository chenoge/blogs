---
title: CSS-object-fit
date: 2018-05-08 15:49:02
tags: [css,object-fit]
---

#  一、定义

``` 
object-fit : css property specifies how the contents of a replaced element should be fitted to the box established by its used height and width.

object-position : css property determines the alignment of the replaced element inside its box.
```

**<font color=#A52A2A size=4 >这里的object实际上指的是replaced element** </font>。

<br/>

# 二、替换元素

- 其内容不受CSS视觉<font color=#A52A2A size=4 >**格式化模型**</font>控制的元素。

  - 比如image, 嵌入的文档(iframe之类)或者applet。
  - 比如，img元素的内容通常会被其src属性指定的图像替换掉。
  - 替换元素通常有其固有的尺寸：一个固有的宽度，一个固有的高度和一个固有的比率。比如一幅位图有固有用绝对单位指定的宽度和高度，从而也有固有的宽高比率。另一方面，其他文档也可能没有固有的尺寸，比如一个空白的html文档。

  

- **CSS渲染模型不考虑<font color=#A52A2A size=4 >替换元素内容</font>的渲染**。这些替换元素的展现独立于CSS。**object, video, textarea, input也是替换元素，audio和canvas在某些特定情形下为替换元素。**



- **使用CSS的content属性插入的对象是匿名替换元素。**

<br/>

<!--more--> 

# 三、object-fit

object-fit具体有5个值：

- **fill**: **默认值**，替换内容拉伸填满整个content box，不保证保持原有的比例
- **contain**: 保持原有尺寸比例，保证替换内容尺寸一定可以在容器里面放得下
- **cover**:保持原有尺寸比例，保证替换内容尺寸一定大于容器尺寸，宽度和高度至少有一个和容器一致
- **none**: 保持原有尺寸比例，同时保持替换内容原始尺寸大小
- **scale-down**: 好像依次设置了none或contain，最终呈现的是尺寸比较小的那个

<br/>

# 四、深入理解

**一个图片，如果没有src，它依然是个替换元素，它在浏览器中的解析依然是正确的。** 



src指向的图片属于替换内容，注意，<font color=#A52A2A size=4 >**替换内容**</font>和<font color=#A52A2A size=4 >**替换元素**</font>是壳子与内容的关系，两者是独立的。



在CSS2.1时代，壳子的实际尺寸（如果没有CSS或HTML设置），则是跟随内容的实际尺寸，因此，网页加载的时候，我们会看到图片占据的高度从0到图片实际高度跳动的过程；



**如果壳子，也就是img有尺寸限制，则替换内容fill拉伸适应于 img替换元素的设定尺寸**。总而言之，壳子与内容的尺寸永远是一样的。于是，我们就会误认为图片就是那个图片，唯一的存在，导致我们理解object-fit的特性表现出现了障碍。 



在CSS3时代，object-fit的世界里，<font color=#A52A2A size=4 >**object-fit控制的永远是替换内容的尺寸表现**</font>，注意，是替换内容的尺寸表现，不是img替换元素。

<br/>

# 五、object-position

object-position要比object-fit单纯的多，就是控制替换内容位置的。默认值是50% 50%。 

与background-position类似，object-position的值类型为<position>类型值。