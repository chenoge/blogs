---
title: js-观察者模式
date: 2018-03-17 16:44:25
tags: [javaScript,观察者模式]
---

观察者模式，定义对象间的一种<font color=#A52A2A size=4 >**一对多的依赖关系**</font>，**当一个对象的状态发生改变时，所有依赖于它的对象都将得到通知**。事实上，只要你曾经在DOM节点上绑定过事件函数，那么你就曾经使用过观察者模式了 

```javascript
document.body.addEventListener('click', function () {
    alert(2);
});
```

但是这只是对观察者模式最简单的使用，在很多场景下我们经常会实现一些自定义事件来满足我们的需求。 

<br/>

```markdown
举个例子：
你去一家公司应聘，谈了一顿下来，hr跟你说:"好了，你回去等通知吧！"。
这个时候，把自己的手机号留给hr，然后等他给你打电话。
那么这个时候，hr就相当于一个发布者，而你就是一个订阅者啦！
```

<br/>

那么一个简单的观察者模式应该怎么实现呢？ 

1. 要指定一个发布者；
2. 给发布者添加一个缓存列表，用于存放回调函数以便通知订阅者；
3. 最后发布消息的时候，发布者会遍历这个缓存列表，依次触发订阅者回调函数；

```javascript
var event = {}; //发布者（hr）
event.clietList = []; //发布者的缓存列表（应聘者列表）

event.listen = function(fn) { //增加订阅者函数
    this.clietList.push(fn);
};

event.trigger = function() { //发布消息函数
    for (var i = 0; i < this.clietList.length; i++) {
        var fn = this.clietList[i];
        fn.apply(this, arguments);
    }
};

event.listen(function(time) { //某人订阅了这个消息
    console.log('正式上班时间：' + time);
});

event.trigger('2016/10', yes); //发布消息
//输出 正式上班时间:2016/10
```

<br/>

<!--more--> 

到这里，我们已经实现了一个最简单的观察者模式了！ 

但是上面的函数其实存在一个问题，那就是发布者没办法选择自己要发布的消息类型！比如这家公司同时在招php，web前端，如果使用上面的函数就没办法区分职位了！只能一次性把全部订阅者都发送一遍消息。

对上面的代码进行改写：

```javascript
var event = {}; //发布者（hr）
event.clietList = []; //发布者的缓存列表（应聘者列表）

event.listen = function(key, fn) { //增加订阅者函数
    if (!this.clietList[key]) {
        this.clietList[key] = [];
    }
    this.clietList[key].push(fn);
};

event.trigger = function() { //发布消息函数
    var key = Array.prototype.shift.call(arguments);
    fns = this.clietList[key];
    for (var i = 0; i < fns.length; i++) {
        var fn = fns[i];
        fn.apply(this, arguments);
    }
};

event.listen('web前端', function(time) { //小强订阅了这个消息。
    console.log('姓名：小强');
    console.log('正式上班时间：' + time);
});

event.listen('php', function(time) { //大大强订阅了这个消息
    console.log('姓名：大大强');
    console.log('正式上班时间：' + time);
});

//发布者发布消息
event.trigger('web前端', '2016/10'); //姓名：小强   正式上班时间：2016/10  
event.trigger('php', '2016/15'); //姓名：大大强   正式上班时间：2016/15
```

通过添加了一个key，我们实现了对职位的判断。有了订阅事件，我们怎么能少了取消订阅事件呢？ 

```javascript
event.remove = function(key, fn) {
    var fns = this.clietList[key];
    if (!fns) {
        return false;
    }

    if (!fn) { //如果没有传入fn回调函数，直接取消key对应消息的所有订阅
        this.clietList[key] = [];
    } else {
        for (var i = 0; i < fns.length; i++) { //遍历回调函数列表
            var _fn = fns[i];
            if (_fn === fn) {
                fns.splice(i, 1); //删除订阅者的回调函数
            }
        }
    }
};

//这时候必须指定回调函数，否则无法在remove函数中进行对比删除。
event.listen('web前端', fn1 = function(time) { //小强订阅了这个消息。
    console.log('姓名：小强');
    console.log('正式上班时间：' + time);
});

event.listen('web前端', fn2 = function(time) { //大大强订阅了这个消息
    console.log('姓名：大大强');
    console.log('正式上班时间：' + time);
});

event.remove('web前端', fn1);
//发布者发布消息

event.trigger('web前端', '2016/10');
//输出 姓名：大大强   正式上班时间：2016/10
```

对上面代码进行改进，创建一个全局对象来实现观察者模式，使用闭包实现私有变量，仅暴露必须的API给使用者： 

```javascript
var event = (function() {
    var clietList = []; //发布者的缓存列表（应聘者列表）

    var listen = function(key, fn) { //增加订阅者函数
        if (!this.clietList[key]) {
            this.clietList[key] = [];
        }
        this.clietList[key].push(fn);
    };

    var trigger = function() { //发布消息函数
        var key = Array.prototype.shift.call(arguments),
            fns = this.clietList[key];
        for (var i = 0; i < fns.length; i++) {
            var fn = fns[i];
            fn.apply(this, arguments);
        }
    };

    var remove = function(key, fn) {
        var fns = this.clietList[key];
        if (!fns) {
            return false;
        }
        if (!fn) { //如果没有传入fn回调函数，直接取消key对应消息的所有订阅
            this.clietList[key] = [];
        } else {
            for (var i = 0; i < fns.length; i++) { //遍历回调函数列表
                var _fn = fns[i];
                if (_fn === fn) {
                    fns.splice(i, 1); //删除订阅者的回调函数
                }
            }
        }
    };

    return {
        listen: listen,
        trigger: trigger,
        remove: remove
    }

})();
```

