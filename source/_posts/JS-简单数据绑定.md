---
title: JS-简单数据绑定-MVC
date: 2018-01-20 21:25:19
tags: [javaScript,数据绑定]
---

# 一、视图层V

```html
<div>
    <p>你好，<span id='nickName'></span></p>
    <div id="introduce"></div>
</div>
```

<br/>

# 二、视图控制器C

```javascript
var userInfo = {};


Object.defineProperty(userInfo, "nickName", {
    get: function() {
        return document.getElementById('nickName').innerHTML;
    },
    set: function(nick) {
        document.getElementById('nickName').innerHTML = nick;
    }
});


Object.defineProperty(userInfo, "introduce", {
    get: function() {
        return document.getElementById('introduce').innerHTML;
    },
    set: function(introduce) {
        document.getElementById('introduce').innerHTML = introduce;
    }
})
```

<br/>

# 三、数据M

```javascript
userInfo.nickName = "xxx";
userInfo.introduce = "我是xxx，我来自云南，..."
```

