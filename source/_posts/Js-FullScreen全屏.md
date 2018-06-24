---
title: Js-FullScreen全屏
date: 2017-10-22 19:47:58
tags: [FullScreen,全屏]
---

# 零、禁止全屏

```html
<html>

<body onkeydown="noFullScreen(event)">
    <script type="text/javascript">
    function noFullScreen(e) {
        if (e.keyCode === 122) {
            e.preventDefault()
        }
    }
    </script>
</body>

</html>
```

```javascript
document.addEventListener("webkitfullscreenchange", function () {
    document.webkitCancelFullScreen()
}, false)
```

<br/>

<!--more-->

 

# 一、进入全屏

```javascript
// 找到支持的方法, 使用需要全屏的 element 调用  
function launchFullScreen(element) {
    if (element.requestFullscreen) {
        element.requestFullscreen()
    } else if (element.mozRequestFullScreen) {
        element.mozRequestFullScreen()
    } else if (element.webkitRequestFullscreen) {
        element.webkitRequestFullscreen()
    } else if (element.msRequestFullscreen) {
        element.msRequestFullscreen()
    }
}

launchFullScreen(document.documentElement) // 整个页面 
launchFullScreen(document.getElementById("videoElement")) // 某个元素
```

<br/>

# 二、退出全屏

```javascript
// 退出 fullscreen  
function exitFullscreen() {
    if (document.exitFullscreen) {
        document.exitFullscreen()
    } else if (document.mozExitFullScreen) {
        document.mozExitFullScreen()
    } else if (document.webkitExitFullscreen) {
        document.webkitExitFullscreen()
    }
}
```

<br/>

# 三、全屏事件

```javascript
document.addEventListener("fullscreenchange", function() {}, false) 
document.addEventListener("mozfullscreenchange", function() {}, false) 
document.addEventListener("webkitfullscreenchange", function() {}, false) 
document.addEventListener("msfullscreenchange", function() {}, false)
```

<br/>

# 四、全屏样式

```css
	html:-moz-full-screen {
	    background: red;
	}


	html:-webkit-full-screen {
	    background: red;
	}


	html:fullscreen {
	    background: red;
	}
```

