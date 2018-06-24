---
title: Js-webWorker多线程
date: 2017-10-23 19:41:41
tags: [javaScript,webWorker]
---

# 一、概念

工作线程（webWorker）允许JavaScript创建多个线程，但是子线程完全受主线程控制，且**不得操作DOM**

```html
<!DOCTYPE html>
<html>

<head>
    <title>worker</title>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    <script>
    function init() {
        //创建一个Worker对象，并向它传递将在新线程中执行的脚本url
        var worker = new Worker('worker.js')
        //接收worker传递过来的数据
        worker.onmessage = function(event) {
            document.getElementById('result').innerHTML += event.data + "<br/>"
        }
    }
    </script>
</head>

<body onload="init()">
    <div id="result"></div>
</body>

</html>
```

```javascript
// worker.js
var i = 0
function timedCount() {
    for (var j = 0, sum = 0; j < 100; j++) {
        for (var i = 0; i < 100000000; i++) {
            sum += i
        }
    }
    //将得到的sum发送回主线程
    postMessage(sum)
}
postMessage('Before computing, ' + new Date())
timedCount()
postMessage('After computing, ' + new Date())
```

<br/>

<!--more-->

# 二、api

1、postMessage(data)  子线程与主线程之间互相通信使用方法，传递的data为任意值。

2、terminate()  主线程中终止worker，此后无法再利用其进行消息传递。

3、onmessage  当有消息发送时，触发该事件。消息发送是双向的，消息内容可通过data来获取。

4、onerror  出错处理。且错误消息可以通过e.message来获取。

```html
<!DOCTYPE html>

<head>
    <title>worker</title>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    <script>
    function init() {
        var worker = new Worker('worker.js')
        setInterval(function() {
            worker.postMessage({ name: 'monkey' })
        }, 100)
        worker.onmessage = function(event) {
            document.getElementById('result').innerHTML += event.data + "<br/>"
            worker.terminate()
        }
    }
    </script>
</head>

<body onload="init()">
    <div id="result"></div>
</body>

</html>
```

```javascript
onmessage = function(event){
     postMessage(event.data.name)
}
```

