---
title: js-单例模式
date: 2018-03-18 16:35:04
tags: [javaScript,单例模式]
---

#### 单体模式提供了一种将代码组织为一个逻辑单元的手段，这个逻辑单元中的代码可以通过单一变量进行访问。单体模式的优点是：

- 可以用来划分命名空间，减少全局变量的数量 

- 使用单体模式可以使代码组织的更为一致，使代码容易阅读和维护 

- 可以被实例化，且实例化一次

<br/>

```javascript
// 单体模式
var CreateDiv = function(html) {
    this.html = html;
    this.init();
}


CreateDiv.prototype.init = function() {
    var div = document.createElement("div");
    div.innerHTML = this.html;
    document.body.appendChild(div);
};


// 代理实现单体模式
var ProxyMode = (function() {
    var instance;
    return function(html) {
        if (!instance) {
            instance = new CreateDiv("我来测试下");
        }
        return instance;
    }
})();


var a = new ProxyMode("aaa");
var b = new ProxyMode("bbb");
```

