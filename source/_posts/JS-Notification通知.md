---
title: JS-Notification通知
date: 2017-10-25 19:37:19
tags: [Notification]
---

```html
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8">
    <title>html5桌面通知</title>
</head>

<body>
    <input type="button" value="开启桌面通知" onclick="showDeskTopNotice('','HTML5桌面消息');">
    <script>
    function showDeskTopNotice(title, msg) {
        var Notification = 
            window.Notification 
        || window.mozNotification 
        || window.webkitNotification;
        
        if (Notification) {
            Notification.requestPermission(function(status) {
                // status默认值'default'等同于拒绝 
                // 'denied' 意味着用户不想要通知 
                // 'granted' 意味着用户同意启用通知
                if ("granted" != status) {
                    return;
                } else {
                    var tag = "sds" + Math.random();
                    var notify = new Notification(
                        title, {
                            dir: 'auto',
                            lang: 'zh-CN',
                            //实例化的notification的id
                            tag: tag, 
                            //通知的缩略图
                            icon: 'http://www.yinshuajun.com/static/img/favicon.ico', 
                            body: msg //通知的具体内容
                        }
                    );
                    notify.onclick = function() {
                            //如果通知消息被点击,通知窗口将被激活
                            window.focus();
                        },
                        notify.onerror = function() {
                            console.log("HTML5桌面消息出错！！！");
                        };
                    notify.onshow = function() {
                        setTimeout(function() {
                            notify.close();
                        }, 2000)
                    };
                    notify.onclose = function() {
                        console.log("HTML5桌面消息关闭！！！");
                    };
                }
            });
        } else {
            console.log("您的浏览器不支持桌面消息");
        }
    };
    showDeskTopNotice("", "HTML5桌面消息")
    </script>
</body>

</html>
```

