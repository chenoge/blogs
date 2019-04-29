---
title: JS-自定义事件
date: 2018-03-26 22:24:09
tags: [javaScript]
---

```javascript
// 通过document.createEvent 方法创建
var event = document.createEvent(type)

// 通过initEvent或其他的初始化方法
event.initEvent(type, bubbles, cancelable);

// 向一个指定目标派发一个事件
target.dispatchEvent(event)
```

- `event` 就是被创建的 [Event](https://developer.mozilla.org/zh-CN/docs/DOM/event) 对象
- `type` 是一个字符串，表示要创建的事件类型
- 事件类型可能包括`UIEvents、MouseEvents、MutationEvents、HTMLEvents`
- 请查看 [Notes](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/createEvent#Notes) 章节获取详细信息 

<!--more-->

<br/>



```javascript
// 自定义build事件
var event = document.createEvent('Event');
event.initEvent('build', true, true);
elem.dispatchEvent(event);
```

```javascript
// 手动触发StorageEvent事件
var storagEvent = document.createEvent("StorageEvent");
storagEvent.initStorageEvent('storage', false, false, key, oldval, newval, url, storage);
window.dispatchEvent(se);
```

