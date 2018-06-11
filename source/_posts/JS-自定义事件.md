---
title: JS-自定义事件
date: 2018-03-26 22:24:09
tags: [javaScript]
---

```javascript
var event = document.createEvent(type)
```

- event 就是被创建的 [Event](https://developer.mozilla.org/zh-CN/docs/DOM/event) 对象.
- type 是一个字符串，表示要创建的事件类型。事件类型可能包括"UIEvents", "MouseEvents", "MutationEvents", 或者 "HTMLEvents"。请查看 [Notes](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/createEvent#Notes) 章节获取详细信息 

```javascript
// 创建事件
var event = document.createEvent('Event');
	
	
// 定义事件名为'build'.
// 事件类型，是否冒泡，是否阻止浏览器的默认行为
event.initEvent('build', true, true);
	
	
// 监听事件
elem.addEventListener('build', function (e) {
    // e.target matches elem
}, false);
	
	
// 触发对象可以是任何元素或其他事件目标
elem.dispatchEvent(event);
```

