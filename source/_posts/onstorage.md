---
title: onstorage
date: 2019-04-29 11:14:34
tags: [onstorage]
---

#### 触发时机

`Storage` 对象发生变化时，`StorageEvent`事件会触发

- `创建/更新/删除`数据项时，重复设置**相同的键值**不会触发该事件
- `Storage.clear() `方法至多**触发一次**该事件
- 在**同一个页面**内发生的改变不会起作用，在相同域名下的**其他页面**发生的改变才会起作用

```javascript
window.addEventListener('storage', function(e) {
    document.querySelector('.my-key').textContent = e.key;
    document.querySelector('.my-old').textContent = e.oldValue;
    document.querySelector('.my-new').textContent = e.newValue;
    document.querySelector('.my-url').textContent = e.url;
    document.querySelector('.my-storage').textContent = e.storageArea;
});
```