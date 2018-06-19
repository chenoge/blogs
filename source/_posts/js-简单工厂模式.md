---
title: js-简单工厂模式
date: 2018-03-18 16:38:48
tags: [javaScript,工厂模式]
---

# 一、工厂模式

```javascript
function createBlog(name, url) {
    let obj = new Object();
    obj.name = name;
    obj.url = url;
    obj.sayUrl= function() {
        alert(this.url);
    }
    return obj;
}
var blog1 = createBlog('wuyuchang', 'http://www.jb51.net/');
```

<br/>

# 二、构造函数模式

```javascript
function Blog(name, url) {
    this.name = name;
    this.url = url;
    this.alertUrl = function() {
        alert(this.url);
    }
}

let blog = new Blog('wuyuchang', 'http://www.jb51.net/');
```

<br/>

# 三、原型模式

```javascript
function Blog() {}

Blog.prototype.name = 'wuyuchang';

Blog.prototype.url = 'http://tools.jb51.net/';

Blog.prototype.friend = ['fr1', 'fr2', 'fr3', 'fr4'];

Blog.prototype.alertInfo = function() {
    alert(this.name + this.url + this.friend);
}
```

<br/>

# 四、混合模式（原型模式 + 构造函数模式）

```javascript
function Blog(name, url, friend) {
    this.name = name;
    this.url = url;
    this.friend = friend;
}

Blog.prototype.alertInfo = function() {
    alert(this.name + this.url + this.friend);
}
```

<br/>

# 五、动态原型模式

```javascript
function Blog(name, url) {
    this.name = name;
    this.url = url;
    if (typeof this.alertInfo !== 'function') {

        Blog.prototype.alertInfo = function() {
            alert(thia.name + this.url);
        }

    }
}

var blog = new Blog('wuyuchang', 'http://tools.jb51.net');
```





​			