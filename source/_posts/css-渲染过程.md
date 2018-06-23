---
title: css-渲染过程
date: 2018-03-22 18:47:33
tags: [css,渲染]
---

# 一、浏览器渲染原理

关于浏览器工作原理之前有一篇非常出名的文章《[浏览器的工作原理：新式网络浏览器幕后揭秘](http://www.html5rocks.com/zh/tutorials/internals/howbrowserswork/)》。文章详细阐述了浏览器工作原理，下面用两张图来分别描述Firefox和Chrome浏览器对Web页面的渲染过程。

<br/>

# 二、Chrome渲染过程

![](css-渲染过程\css-animation-1.png)

有关于Chrome浏览器渲染的详细内容，可以参考《[图解浏览器渲染过程 - 基于Webkit/Blink内核Chrome浏览器](https://github.com/abcrun/abcrun.github.com/issues/17)》一文。 

<br/>

# 三、Firefox渲染过程

![](css-渲染过程\css-animation-2.jpeg)

特别声明：接下来的内容都是针对于Chrome浏览器进行讨论。 

<br/>

<!--more--> 

# 四、Chrome渲染部分的实际含义

从上面的流程图中不难看出，Chrome渲染主要包括`Parse Html`、`Recalculate Style`、`Layout`、`Rasterizer`、`Paint`、`Image Decode`、`Image Resize`和`Composite Layers`等。简单了解一下其含义，以便后续内容的更好理解。 

#### 1、Parse Html

发送一个http请求，获取请求的内容，然后解析HTML的过程。

有一个经典的前端面试题：[当你在浏览器中输入google.com并且按下回车之后发生了什么？](https://github.com/skyline75489/what-happens-when-zh_CN) 这个面试题或许能帮助大家更好的理解Parse Html，甚至是浏览器渲染的其他几个部分。

<br/>

#### 2、Recalculate Style

重新计算样式，它计算的是Style，和Layout做的事情完全不同。**Layout计算的是一个元素绝对的位置和尺寸**，或者说是“Compute Layout”。

Recalculate被触发的时候做的事情就是**处理JavaScript给元素设置的样式而已**。Recalculate Style会计算Render树（渲染树）,然后从根节点开始进行页面渲染，将**CSS附加到DOM**上的过程。

任何企图改变元素样式的操作都会触发Recalculate。同Layout一样，它也是在**JavaScript执行完成后才触发的**。

<br/>

#### 3、Layout

计算页面上的布局，即元素在文档中的位置及大小。正如前面所述，Layout计算的是布局位置信息。**任何有可能改变元素位置或大小的样式都会触发这个Layout事件**。

触发Layout的属性非常的多，如果想了解什么属性会触发Layout事件，可以在[CSS Triggers](https://csstriggers.com/)网站查阅。下图截了一部分：

![](css-渲染过程\css-animation-3.png)

<br/>

#### 4、Rasterizer

光栅化，一般的安卓手机都会进行光栅化，光栅主要是针对图形的一个栅格化过程。低端手机在这部分耗时还是蛮多的。

 <br/>

#### 5、Paint

**页面上显示东西有任何变动都会触发Paint**。包括拖动滚动条，鼠标选择中文字等这些完全不改变样式，只改变显示结果的动作都会触发Paint。

Paint的工作就是把文档中用户可见的那一部分展现给用户。Paint是把Layout和Recalculate的计算的结果直接在浏览器视窗上绘制出来，它并不实现具体的元素计算。

 <br/> 

#### 6、Image Decode

图片解码，将图片解析到浏览器上显示的过程。

 <br/> 

#### 7、Image Resize

图片的大小设置，图片加载解析后，若发现图片大小并不是实际的大小（CSS改变了宽度），则需要Resize。Resize越大，耗时越久，所以尽量以图片的原始大小输出。

<br/>

#### 8、Composite Layers

最后**合并图层**，输出页面到屏幕。浏览器在渲染过程中会将一些含有特殊样式的DOM结构绘制于其他图层，有点类似于PhotoShop的图层概念。一张图片在PotoShop是由多个图层组合而成，而浏览器最终显示的页面实际也是有多个图层构成的。

下面这些因素都会导致**新图层的创建**：

- 进行3D或者透视变换的CSS属性
- 使用硬件加速视频解码的<video>元素
- 具有3D（WebGL）上下文或者硬件加速的2D上下文的<canvas>元素
- 组合型插件（即Flash）
- 具有有CSS透明度动画或者使用动画式Webkit变换的元素
- 具有硬件加速的CSS滤镜的元素

有关于Composite方面的深入剖析，可以阅读《[无线性能优化：Composite](http://taobaofed.org/blog/2016/04/25/performance-composite/)》一文。

<br/>

# 五、像素渲染流水线

通过前面的介绍，在屏幕上最终呈现的页面，是类似于图层一样合并输出到屏幕上的。其实所写的Web页面最终以像素的形式在浏览器屏幕上呈现。这样一来，我们需要理解所写的页面代码是如何被转换成屏幕上显示的像素。这个转换过程可以归纳为这样的一个流水线，主要包含五个关键步骤： 

![](css-渲染过程\css-animation-4.jpg)

- JavaScript：一般来说，我们会使用JavaScript来实现一些视觉变化的效果。比如CSS Animation、Transition和Web Animation API。 

- Style：计算样式。这个过程是根据CSS选择器，对每个DOM元素匹配对应的CSS样式。这一步结束之后，就确定了每个DOM元素上应该应用什么CSS样式规则。 (**Recalculate Style**)

- Layout：布局。上一步确定了每个DOM元素的样式规则，这一步就是具体计算每个DOM元素**最终在屏幕上显示的大小和位置**。Web页面中元素的布局是相对的，因此一个元素的布局发生变化，会联动地引发其他元素的布局发生变化。比如，<body>元素的width变化会影响其后代元素的宽度。因此，对于浏览器而言，布局过程是经常发生的。 (**Compute Layout**)

- Paint：绘制。本质上就是填充像素的过程。包括绘制文字、颜色、图像、边框和阴影等，也就是一个DOM元素所有的可视效果。一般来说，这个绘制过程是在多个层上完成的。 

- Composite：**渲染层合并**。前面也说过，对于页面中DOM元素的绘制是在多个层上进行的。在每个层上完成绘制过程之后，浏览器会将所有层按照合理的顺序合并成一个图层，然后在屏幕上呈现。对于有位置重叠的元素的页面，这个过程尤其重要，因为一量图层的合并顺序出错，将会导致元素显示异常。

上述过程的每一步都有可能会发生，因此一定要弄清楚自己的代码将会运行在哪一步。

虽然在理论上，页面的每一帧都是结过上述的流水线处理之后渲染出来的，但并不意味着页面每一帧的渲染都需要经过上述五个步骤的处理。实际上，对视觉变化效果的一个帧的渲染，有三种常用的流水线。

<br/>

#### 1、JavaScript/CSS =>计算样式=>布局=>绘制=>渲染层合并

![](css-渲染过程\css-animation-4.jpg)

如果你修改一个DOM元素的“Layout”属性，也就是改变了元素的样式（比如width、height或者position等），那么浏览器会检查哪些元素需要重新布局，然后对页面激发一个reflow（重排）过程完成重新布局。被reflow（重排）的元素，接下来也会激发绘制过程，最后激发渲染层合并过程，生成最后的画面。

reflow又叫重排,是指浏览器计算页面的全部或部分布局所做的处理。reflow必定会引发重绘，这对于Web的性能影响是极大的。

<br/>

#### 2、JavaScript/CSS => 计算样式 =>绘制 =>渲染层合并

![](css-渲染过程\css-animation-5.jpg)

如果你修改一个DOM元素的“Paint Only”属性，比如背景图片、文字颜色或阴影等，这些属性不会影响页面的布局，因此浏览器会在完成样式计算之后，跳过布局过程，只会绘制和渲染层合并过程。 

<br/>

#### 3、JavaScript/CSS => 计算样式 =>渲染层合并

![像素渲染流水线](css-渲染过程\css-animation-6.jpg)

如果你修改一个非样式且非绘制的CSS属性，那么浏览器会在完成样式计算之后，跳过布局和绘制的过程，直接做渲染层合并。这种方式在性能上是最理想的，对于动画和滚动这种负荷很重的渲染，我们要争取使用第三种渲染过程。

 

通过前面这么多的内容介绍，我们可以得知，影响Web性能主要过程包括Layout、Paint和Composite。那么对于CSS Animation而言，我们的所有操作都是通过CSS的样式控制动画，言外之意，只要是会触发Layout、Paint和Composite的CSS属性都会直接影响动画的性能。在CSS中所有影响Layout、Paint和Composite的属性都可以通过[CSS Triggers](https://csstriggers.com/)**网站查阅。那么如何避免达到前面所述的，整个动画尽量避开重排和重绘，只做渲染层合并呢？暂且先不讨论，把这部分放到最后面来讨论。接下来接着先看看其他相关的知识点。



<br/>

# 六、渲染性能

在理解渲染性能之前，我们有必要先了解前面提到的两个概念重排（也就是回流）和重绘。因为这两者与前面介绍的像素渲染流水线中的Layout和Paint都有关系，而且Layout和Paint对性能的渲染又有莫大的关系。 

#### 1、Reflow（重排）

**Reflow（重排）指的是计算页面布局（Layout）**。某个节点Reflow时会重新计算节点的尺寸和位置，而且还有可能触其后代节点Reflow。在这之后再次触发一次Repaint（重绘）

当Render Tree中的一部分（或全部）因为元素的尺寸、布局、隐藏等改变而需要重新构建。这就称为回流，每个页面至少需要一次回流，就是页面第一次加载的时候。

在Web页面中，很多状况下会导致回流：

- 调整窗口大小
- 改变字体
- 增加或者移除样式表
- 内容变化
- 激活CSS伪类
- 操作CSS属性
- JavaScript操作DOM
- 计算offsetWidth和offsetHeight
- 设置style属性的值
- CSS3 Animation或Transition

<br/>

#### 2、Repaint（重绘）

**Repaint（重绘）或者Redraw遍历所有节点，检测节点的可见性、颜色、轮廓等可见的样式属性，然后根据检测的结果更新页面的响应部分**。 

当Render Tree中的一些元素需要更新属性，而这些属性只是影响元素的外观、风格、而不会影响布局的。就是重绘。 将重排和重绘的介绍结合起来，不难发现：重绘（Repaint）不一定会引起回流（Reflow重排），但回流必将引起重绘（Repaint）。 

既然如此，那么什么情况之下会触发浏览器的Repaint和Reflow呢？

- 页面首次加载
- DOM元素添加、修改(内容)和删除(Reflow + Repaint)
- 仅修改DOM元素的颜色(只有Repaint，因为不需要调整布局)
- 应用新的样式或修改任何影响元素外观的属性
- Resize浏览器窗口和**滚动页面**
- **读取元素的某些属性**(offsetLeft、offsetTop、offsetHeight、offsetWidth、getComputedStyle()等)

可以说Reflow和Repaint都很容易触发，而它们的触发对性能的影响都非常大，但非常不幸的是，我们无法完全避免，只能尽量不去触发浏览器的Reflow和Repaint。 

从前面的内容可以了解到，Reflow和Repaint对性能影响很大，那么具体哪些点会影响到渲染性能呢？

<br/>

#### 3、影响Layout的属性

当你改变页面上某个元素的时候，浏览器需要做一次重新布局的操作，这次操作会包括计算受操作影响所有元素的几何数，比如每个元素的位置和尺寸。如果你修改了html这个元素的width属性，那么整个页面都会被重绘。

由于元素相覆盖，相互影响，稍有不慎的操作就有可能导致一次自上而下的布局计算。所以我们在进行元素操作的时候要一再小心尽量避免修改这些重新布局的属性。

具体有关于会影响Layout的CSS属性可以在[CSS Triggers](https://csstriggers.com/)网站中查阅。

<br/>

#### 4、影响Repaint的属性

有些属性的修改不会触发重排，但会触Repaint（重绘）,现代浏览器中主要的绘制工作主要用光栅化软件来完成。所以重新会制的元素是否会很大程度影响你的性能，是由这个元素和绘制层级的关系来决定的，如果这个元素盖住的元素都被重新绘制，那么代价自然就相当地大。

 

具体有关于会影响Layout的CSS属性可以在[CSS Triggers](https://csstriggers.com/)网站中查阅。

如果你在动画里面使用了上述某些属性，导致重绘，这个元素所属的图层会被重新上传到GPU。在移动设备上这是一个很昂贵耗资源的操作，因为移动设备的CPU明显不如你的电脑，这也意味着绘制的工作会需要更长的时间；而上传线CPU和GPU的带宽并非没有限制，所以重绘的纹理上传就自然需要更长的时间。

 

[CSS Triggers](https://csstriggers.com/)网站中可以得知哪些属性会触发重排、哪些属性会触发重绘以及哪些属性会触合成。但并不是CSS中所有的属性都可以用于CSS Animation和Transition中的。在W3C官方规范中明确定了哪些CSS属性可以用于[Animation](https://www.w3.org/TR/css3-transitions/#animatable-css)和[Transition](https://www.w3.org/TR/css3-transitions/#transition-property-property)中。[@Rodney Rehm](http://rodneyrehm.de/)还对这些[属性做过一个兼容测试](http://thewebevolved.com/support/animation/properties/)。如果你想深入的了解这方面的知识，建议您阅读下面两篇文章：

- [CSS animatable properties](http://oli.jp/2010/css-animatable-properties/)
- [Thank God We Have A      Specification!](https://www.smashingmagazine.com/2013/04/css3-transitions-thank-god-specification/)

如此一来，我们知道可用于CSS Animation或者Transition的CSS属性之后，再配合[CSS Triggers](https://csstriggers.com/)网站，可以轻易掌握哪些CSS属性会触发重排、重绘和合成等。虽然无法避免，但我们可以尽量控制。 

<br/>

# 七、性能优化

如果我们知道浏览器是如何渲染一个页面的，并且去优化渲染过程中的关键步骤，不是是就能事半功倍呢？

有关于这部分的介绍，建议大家阅读《[渲染性能](http://www.wdshare.org/article/5770ed9753c50d1a18f64a97)》。

![](css-渲染过程\css-animation-6.jpg)

在像素渲染流水线中，得知，如果我们能幸运的避免Layout和Paint，那么性能是最好的，言外之意，动画性能也将变得最佳。那么在CSS中可能通过不同的方式来创建新图层。其实这也就是大家常说的，通过CSS的属性来触发GPU加速。浏览器会为此元素单独创建一个“层”。当有单独的层之后，此元素的Repaint操作将只需要更新自己，不用影响到别人。你可以将其理解为局部更新。所以开启了硬件加速的动画会变得流畅很多。

 

为什么开启硬件加速动画就会变得流畅，那是因为**开启硬件加速动画的元素**都有一个**独立的Render进程**。Render进程中包含了主线程和合成线程，主线程负责：

- JavaScript的执行
- CSS样式计算
- 计算Layout
- 将页面元素绘制成位图(Paint)
- 发送位图给合成线程

合成线程则主要负责： 

- 将位图发送给GPU
- 计算页面的可见部分和即将可见部分(滚动)
- 通知GPU绘制位图到屏幕上(Draw)

我们可以得到一个大概的浏览器线程模型： 

![](css-渲染过程\css-animation-7.png)

我们可以将页面绘制的过程分为三个部分：Layout、Paint和合成。Layout负责计算DOM元素的布局关系，Paint负责将DOM元素绘制成位图，合成则负责将位图发送给GPU绘制到屏幕上（如果有transform、opacity等属性则通知GPU做处理）。

 

GPU加速其实是一直存在的，而如同translate3D这种hack只是为了让这个元素生成独立的 GraphicsLayer ， 占用一部分内存，但同时也会在动画或者Repaint的时候不会影响到其他任何元素，对高刷新频率的东西，就应该分离出单独的一个 GraphicsLayer。

 

GPU对于动画图形的渲染处理比CPU要快。

RenderLayer 树，满足以下任意一点的就会生成独立一个 RenderLayer

- 页面的根节点的RenderObject
- 有明确的CSS定位属性（relative，absolute或者transform）
- 是透明的
- 有CSS overflow、CSS alpha遮罩（alpha mash）或者CSS reflection
- 有CSS 滤镜（fliter）
- 3D环境或者2D加速环境的canvas元素对应的RenderObject
- video元素对应的RenderObject

<br/>

每个RenderLayer 有多个 GraphicsLayer 存在 

- 有3D或者perspective transform的CSS属性的层
- 使用加速视频解码的video元素的层
- 3D或者加速2D环境下的canvas元素的层
- 插件，比如flash（Layer is used for a      composited plugin）
- 对opacity和transform应用了CSS动画的层
- 使用了加速CSS滤镜（filters）的层
- 有合成层后代的层
- 同合成层重叠，且在该合成层上面（z-index）渲染的层

<br/>

每个GraphicsLayer 生成一个 GraphicsContext, 就是一个位图，传送给GPU，由GPU合成放出。

那么就是说，GraphicsLayer过少则每次repaint大整体的工作量巨大，而过多则repaint小碎块的次数过多。这种次数过多就称为 层数爆炸 ，为了防止这个爆炸 Blink 引擎做了一个特殊处理。

有关于这部分内容的详细介绍，可以阅读《[无线性能优化：Composite](http://taobaofed.org/blog/2016/04/25/performance-composite/)》一文。

<br/>

扯了这么多，我们可以稍微总结一下下：

不是所有属性动画消耗的性能都一样，其中消耗最低的是transform和opacity两个属性（当然还有会触发Composite的其他CSS属性），其次是Paint相关属性。所以在制作动画时，建议使用transform的translate替代margin或position中的top、right、bottom和left，同时使用transform中的scaleX或者scaleY来替代width和height。

<br/>

为了确保页面的流程，必须保证60fps内不发生两次渲染树更新，比如下图，16ms内只发生如下几个操作则是正常及正确的：

![](css-渲染过程\css-animation-8.png)

页面滚动时，需要避免不必要的渲染及长时间渲染。其中不必要的渲染包括： 

- position:fixed;。fixed定位在滚动时会不停的进行渲染，特别是页面顶部有一个fixed，页面底部有个类似返回顶部的fixed，则在滚动时会对整个页面进行渲染，效率非常低。可以通过transform: translateZ(0)或者transform: translate3d(0,0,0)来解决 

- overflow:scroll。前面说了，而在滚动也会触发Repaint和Reflow。在调试过程中注意到一个有趣的现象，有时打开了页面并不会导致crash，但快速滑动的时候却会。由于crash是页面本身内存占比过高，只要优化了页面的内存占用，滑动自然也不会是很大的问题。无论你在什么时候滑动页面，页面滚动都是一个不断重新组合重新绘制的过程。所以减少渲染区域在滚动里就显得非常重要。 

- CSS伪类触发。有些CSS伪类在页面滚动时会不小心触发到。比如:hover效果有box-shadow、border-radius等比较耗时的CSS属性时，建议页面滚动时，先取消:hover效果，滚动停止后再加上:hover效果。这个可以通过在外层添加类名进行控制。但添加类名、删除类名也会改变元素时，浏览器就会要重新做一次计算和布局。所以千万要小心这种无意触发重新布局的操作，有的时候可能不是动画，但去付出的代价要比做一个动画更加昂贵。也就是说classname变化了，就一定会出现一次rendering计算，如果一定需要这么做，那可以使用 classlist 的方法。 

- touch事件的监听

<br/>

长时间渲染包括： 

- 复杂的CSS 

- Image Decodes：特别是图片的Image Decodes及Image Resize这两个过程在移动端是非常耗时的 

- Large Empty Layers: 大的空图层