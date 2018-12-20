---
title: Performance
date: 2018-12-20 20:07:46
tags: [Performance,首屏时间,白屏时间]
---

### 一、performance 

`performace`允许访问当前页面性能相关的信息，主要功能都是由`Performance Timeline API`、`the Navigation Timing API`、`the User Timing API`、  `the Resource Timing API`提供的。

##### 1、performance.timing，提供了各种与浏览器处理相关的时间数据 

| 名称                       | 作用（这里所有时间戳都代表UNIX毫秒时间戳）                   |
| -------------------------- | ------------------------------------------------------------ |
| connectEnd                 | 浏览器与服务器之间的连接建立时的时间戳，连接建立指的是所有握手和认证过程全部结束 |
| connectStart               | HTTP请求开始向服务器发送时的时间戳，如果是持久连接，则等同于fetchStart。 |
| domComplete                | 当前网页DOM结构生成时，也就是Document.readyState属性变为“complete”,并且相应的readystatechange事件触发时的时间戳。 |
| domContentLoadedEventEnd   | 当前网页DOMContentLoaded事件发生时，也就是DOM结构解析完毕、所有脚本运行完成时的时间戳。 |
| domContentLoadedEventStart | 当前网页DOMContentLoaded事件发生时，也就是DOM结构解析完毕、所有脚本开始运行时的时间戳。 |
| domInteractive             | 当前网页DOM结构结束解析、开始加载内嵌资源时，也就是Document.readyState属性变为“interactive”、并且相应的readystatechange事件触发时的时间戳。 |
| domLoading                 | 当前网页DOM结构开始解析时,也就是Document.readyState属性变为“loading”、并且相应的readystatechange事件触发时的时间戳。 |
| domainLookupEnd            | 域名查询结束时的时间戳。如果使用持久连接，或者从本地缓存获取信息的，等同于fetchStart |
| domainLookupStart          | 域名查询开始时的时间戳。如果使用持久连接，或者从本地缓存获取信息的，等同于fetchStart |
| fetchStart                 | 浏览器准备通过HTTP请求去获取页面的时间戳。在检查应用缓存之前发生。 |
| loadEventEnd               | 当前网页load事件的回调函数结束时的时间戳。如果该事件还没有发生，返回0。 |
| loadEventStart             | 当前网页load事件的回调函数开始时的时间戳。如果该事件还没有发生，返回0。 |
| navigationStart            | 当前浏览器窗口的前一个网页关闭，发生unload事件时的时间戳。如果没有前一个网页，就等于fetchStart |
| redirectEnd                | 最后一次重定向完成，也就是Http响应的最后一个字节返回时的时间戳。如果没有重定向，或者上次重定向不是同源的。则为0 |
| redirectStart              | 第一次重定向开始时的时间戳，如果没有重定向，或者上次重定向不是同源的。则为0 |
| requestStart               | 浏览器向服务器发出HTTP请求时（或开始读取本地缓存时）的时间戳。 |
| responseEnd                | 浏览器从服务器收到（或从本地缓存读取）最后一个字节时（如果在此之前HTTP连接已经关闭，则返回关闭时）的时间戳 |
| responseStart              | 浏览器从服务器收到（或从本地缓存读取）第一个字节时的时间戳。 |
| secureConnectionStart      | 浏览器与服务器开始安全链接的握手时的时间戳。如果当前网页不要求安全连接，则返回0。 |
| unloadEventEnd             | 如果前一个网页与当前网页属于同一个域下，则表示前一个网页的unload回调结束时的时间戳。如果没有前一个网页，或者之前的网页跳转不是属于同一个域内，则返回值为0。 |
| unloadEventStart           | 如果前一个网页与当前网页属于同一个域下，则表示前一个网页的unload事件发生时的时间戳。如果没有前一个网页，或者之前的网页跳转不是属于同一个域内，则返回值为0。 |

![](Performance\perfomance.png)

**组合值的意义** 

> DNS查询耗时 ：domainLookupEnd - domainLookupStart
>
> TCP链接耗时 ：connectEnd - connectStart
>
> request请求耗时 ：responseEnd - responseStart
>
> 解析dom树耗时 ： domComplete - domInteractive
>
> 白屏时间 ：responseStart - navigationStart
>
> domready时间 ：domContentLoadedEventEnd - navigationStart
>
> onload时间 ：loadEventEnd - navigationStart

<br/>

<!--more-->

##### 2、performance.navagation，呈现了如何导航到当前文档的信息

##### `performance.navagation`有两个属性

- `type`，表示如何导航到当前页面的，主要有4个值
  - type = 0，通过点击链接、书签和表单提交，或者脚本操作，或者在url中直接输入地址访问的
  - type=1，点击刷新或者调用Location.reload()方法访问的
  - type=2，通过历史记录或者前进后退按钮访问的
  - type=255，其他方式访问的
- `redirectCount`，表示到达当前页面之前经过几次重定向

<br/>

##### 3、performance.timeOrigin

表示performance性能测试开始的时间，是一个高精度时间戳（千分之一毫秒） 

<br/>

##### 4、performance.onresourcetimingbufferfull

表示当浏览器资源时间性能缓冲区已满时会触发的回调函数。下面是mdn上关于这个属性的一个demo。这个demo的主要内容是当缓冲区内容满时，调用buffer_full函数。 

```javascript
function buffer_full(event) {
  console.log("WARNING: Resource Timing Buffer is FULL!");
  performance.setResourceTimingBufferSize(200);
}

function init() {
  // Set a callback if the resource buffer becomes filled
  performance.onresourcetimingbufferfull = buffer_full;
}

<body onload="init()">
```

<br/>

##### 5、performance.memory

一个非标准属性，由chrome浏览器提供，这个属性提供了一个可以获取到基本内存使用情况的对象。 

```javascript
memory: {
    jsHeapSizeLimit: 2217857988, // 内存大小限制
    totalJSHeapSize: 27488256, // 可使用的内存
    usedJSHeapSize: 23214320 // JS对象(包括V8引擎内部对象)已占用的内存
}
```

注：`usedJSHeapSize`表示所有被使用的js堆栈内存；`totalJSHeapSize`表示当前js堆栈内存总大小，这表示`usedJSHeapSize`不能大于`totalJSHeapSize`，如果大于，有可能出现了**内存泄漏**。

<br/>

##### 6、performance.getEntries 

```javascript
var resourcesObj = performance.getEntries();
[
    {
        connectEnd: 1057.4999999989814,
        connectStart: 1057.4999999989814,
        decodedBodySize: 4263,
        domainLookupEnd: 1057.4999999989814,
        domainLookupStart: 1057.4999999989814,
        duration: 453.3000000010361,
        encodedBodySize: 4263,
        entryType: "resource",
        fetchStart: 1057.4999999989814,
        initiatorType: "img",
        name: "https://cdn.segmentfault.com/v-5c19e300/page/img/app/appQrcode.png",
        nextHopProtocol: "http/1.1",
        redirectEnd: 0,
        redirectStart: 0,
        requestStart: 1508.2999999976892,
        responseEnd: 1510.8000000000175,
        responseStart: 1509.200000000419,
        secureConnectionStart: 0,
        serverTiming: [],
        startTime: 1057.4999999989814,
        transferSize: 0,
        workerStart: 0
    },
    ...
]
```

返回的是一个对象数组，按`startTime`排序，数组每一个项都是一个对象，这个对象中包含了当前静态资源的加载Timing 。还可以用`mark()`，`measure()`方法自定义添加 。

```javascript
// 标记一个开始点
performance.mark("mySetTimeout-start");

// 等待1000ms
setTimeout(function() {
  // 标记一个结束点
  performance.mark("mySetTimeout-end");

  // 标记开始点和结束点之间的时间戳
  performance.measure(
    "mySetTimeout",
    "mySetTimeout-start",
    "mySetTimeout-end"
  );

  // 获取所有名称为mySetTimeout的measures
  var measures = performance.getEntriesByName("mySetTimeout");
  var measure = measures[0];
  console.log("setTimeout milliseconds:", measure.duration)

  // 清除标记
  performance.clearMarks();
  performance.clearMeasures();
}, 1000);
```



<br/>

### 二、一套性能API标准 

| API                      | 名称         | 功能                                                         |
| ------------------------ | :----------- | ------------------------------------------------------------ |
| Navigation Timing        | 导航计时     | 能够帮助网站开发者检测真实用户数据（RUM），例如带宽、延迟或主页的整体页面加载时间。 |
| Resource Timing          | 资源计时     | 对单个资源的计时，可以对细粒度的用户体验进行检测。           |
| High Resolution Timing   | 高精度计时   | 该API规范所定义的JavaScript接口能够提供精确到微秒级的当前时间，并且不会受到系统时钟偏差或调整的影响。 |
| Page Visibility          | 页面可见性   | 通过这一规范，网站开发者能够以编程方式确定页面的当前可见状态，从而使网站能够更有效地利用电源与CPU。当页面获得或失去焦点时，文档对象的visibilitychange事件便会被触发。 |
| Performance Timeline     | 性能时间线   | 以一个统一的接口获取由Navigation Timing、Resourcing Timing和User Timing所收集的性能数据。 |
| Battery Status           | 电池状态     | 能够检测当前设备的电池状态，例如是否正在充电、电量等级。可以根据当前电量决定是否显示某些内容，对于移动设备来说非常实用。 |
| User Timing              | 用户计时     | 可以对某段代码、函数进行自定义计时，以了解这段代码的具体运行时间。 |
| Beacon                   | 灯塔         | 可以将分析结果或诊断代码发送给服务器，它采用了异步执行的方式，因此不会影响页面中其它代码的运行。 |
| Animation Timing         | 动画计时     | 通过requestAnimationFrame函数让浏览器精通地控制动画的帧数，能够有效地配合显示器的刷新率，提供更平滑的动画效果，减少对CPU和电池的消耗。 |
| Resource Hits            | 资源提示     | 通过html属性指定资源的预加载，例如在浏览相册时能够预先加载下一张图片，加快翻页的显示速度。 |
| Frame Timing             | 帧计时       | 通过一个接口获取与帧相关的性能数据，例如每秒帧数和TTF。      |
| Navigation Error Logging | 错误日志记录 | 通过一个接口存储及获取与某个文档相关的错误记录。             |

<br/>