---
title: JS-addEventListener和on
date: 2017-10-27 19:32:16
tags: [addEventListener,on]
---

# 一、使用on方法绑定事件

```javascript
window.onload = function(){
    var box = document.getElementById("box")
    box.onclick = function(){
        console.log("我是box1")
    }
    
    box.onclick = function(){
        box.style.fontSize = "18px"
        console.log("我是box2")
    }
}
```

- **第一个onclick回调函数会被第二个回调函数覆盖**

<br/>

<!--more-->

# 二、addEventListener绑定事件

```javascript
window.onload = function(){
    var box = document.getElementById("box")
    box.addEventListener("click",function(){
        console.log("我是box1")
    })
    
    box.addEventListener("click",function(){
        console.log("我是box2")
    })
}
```

- **addEventListener允许在同一个事件上，绑定多个函数**
- **既不覆盖on方法绑定的函数，也不覆盖addEventListener绑定的函数**