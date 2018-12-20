---
title: visibilityState
date: 2018-12-20 21:59:14
tags: [document,visibilityState,visibilitychange]
---

#### 一、document.visibilityState

`Page Visibility API` 在`document`对象上，新增了一个`document.visibilityState`属性。该属性返回一个字符串，表示页面当前的可见性状态，共有三个可能的值。

- `hidden`：页面彻底不可见
- `visible`：页面至少一部分可见
- `prerender`：页面即将或正在渲染，处于不可见状态

其中，`hidden`状态和`visible`状态是所有浏览器都必须支持的。`prerender`状态只在支持"预渲染"的浏览器上才会出现，比如 Chrome 浏览器就有预渲染功能，可以在用户不可见的状态下，预先把页面渲染出来，等到用户要浏览的时候，直接展示渲染好的网页。 

<br/>

只要页面可见，哪怕只露出一个角，`document.visibilityState`属性就返回`visible`。只有以下四种情况，才会返回`hidden`。

- 浏览器最小化
- 浏览器没有最小化，但是当前页面切换成了背景页
- 浏览器将要卸载（unload）页面
- 操作系统触发锁屏屏幕

<br/>

注意，`document.visibilityState`属性只针对顶层窗口，内嵌的`<iframe>`页面的`document.visibilityState`属性由顶层窗口决定。使用 CSS 属性隐藏`<iframe>`页面（比如`display: none;`），并不会影响内嵌页面的可见性。 

<!--more-->

#### 二、visibilitychange 事件

只要`document.visibilityState`属性发生变化，就会触发`visibilitychange`事件，通过`document.addEventListener()`方法或`document.onvisibilitychange`属性。

```javascript
document.addEventListener('visibilitychange', function () {
  // 用户离开了当前页面
  if (document.visibilityState === 'hidden') {
    document.title = '页面不可见';
  }

  // 用户打开或回到页面
  if (document.visibilityState === 'visible') {
    document.title = '页面可见';
  }
});
```

<br/>

#### 三、`Page Lifecycle API`([原文](http://www.ruanyifeng.com/blog/2018/11/page_lifecycle_api.html))

`Android`、`iOS` 和最新的 `Windows` 系统可以随时自主地停止后台进程，及时释放系统资源。也就是说，网页可能随时被系统丢弃掉。`Page Visibility API` 只在网页对用户不可见时触发，至于网页会不会被系统丢弃掉，它就无能为力了。



为了解决这个问题，W3C 新制定了一个 [Page Lifecycle API](https://github.com/WICG/page-lifecycle)，统一了网页从诞生到卸载的行为模式，并且定义了新的事件，允许开发者响应网页状态的各种转换。



有了这个 API，开发者就可以预测网页下一步的状态，从而进行各种针对性的处理。Chrome 68 支持这个 API，对于老式浏览器可以使用谷歌开发的兼容库 [PageLifecycle.js](https://github.com/GoogleChromeLabs/page-lifecycle)。

<br/>

网页的生命周期分成六个阶段，每个时刻只可能处于其中一个阶段

![](visibilityState\bg2018110401.png)