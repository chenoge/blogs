---
title: pushState和replaceState
date: 2018-12-24 21:33:52
tags: [pushState,replaceState]
---

` window` 对象通过 `history` 对象提供了对浏览器历史的访问。它暴露了很多有用的方法和属性，允许你在用户浏览历史中向前和向后跳转，同时——从`HTML5`开始——提供了对`history栈`中内容的操作。 

```javascript
window.history.back(); // 与点击浏览器回退按钮的效果相同
window.history.forward(); // 与点击浏览器前进按钮的效果相同
window.history.go(-1); // 通过与当前页面相对位置，来标志某一特定页面
```

<br/>

#### 一、pushState

`pushState()` 需要三个参数: `一个状态对象`， `一个标题 (目前被忽略)`，和 `(可选的) 一个URL`。让我们来解释下这三个参数详细内容：

- **状态对象**
  - 状态对象state是一个JavaScript对象
  - `popstate事件`被触发时，该事件的state属性包含`该历史记录`**状态对象**的**副本**
  - 状态对象可以是能被序列化的任何东西，但有`640k`的大小限制
- **标题**
  - Firefox 目前忽略这个参数，但未来可能会用到。传递一个空字符串在这里是安全的，而在将来这是不安全的。二选一的话，你可以为跳转的state传递一个短标题。
- **URL**
  - 该参数定义了新的历史URL记录
  - 注意，调用 `pushState()` 后浏览器并不会立即加载这个URL
  - 新URL不必须为绝对路径。如果新URL是相对路径，那么它将被作为相对于当前URL处理
  - 新URL必须与当前URL**同源**，否则 `pushState()` 会抛出一个异常。该参数是可选的，缺省为当前URL

```javascript
// http://mozilla.org/foo.html 假设当前url

let stateObj = { foo: "bar" };
history.pushState(stateObj, "page 2", "bar.html");

// 这将使浏览器地址栏显示为 http://mozilla.org/bar.html
// 但并不会导致浏览器加载 bar.html ，甚至不会检查bar.html 是否存在。
```

注： `pushState()` 绝对不会触发 `hashchange` 事件，即使新的URL与旧的URL仅哈希不同也是如此。` vue-router` 底层调用的正是`history.pushState`和`history.replaceState`。

<br/>

<!--more-->

#### 二、replaceState

`history.replaceState()` 的使用与 `history.pushState()` 非常相似，区别在于  `replaceState()`  是`修改了当前的历史记录项`而不是新建一个。 `replaceState()` 的使用场景在于为了响应用户操作，你想要`更新状态对象state`或者`当前历史记录的URL`。 

<br/>

#### 三、获取当前状态

 你可以读取`当前历史记录项的状态对象state`，而不必等待`popstate` 事件， 只需要这样使用`history.state` 属性：  

```javascript
let currentState = history.state;
```

<br/>

#### 四、改变referrer

使用 `history.pushState()` 可以改变`referrer`，它在用户发送 `XMLHttpRequest`请求时在`HTTP头部`使用，改变`state`后创建的 `XMLHttpRequest`对象的`referrer`都会被改变，但**超越不了同源策略**。