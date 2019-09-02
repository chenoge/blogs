---
title: 动态加载
date: 2019-09-02 10:26:37
tags: [import,eval,动态加载]
---

#### 动态加载

##### `import()`

关键字`import`可以像调用函数一样来动态的导入模块。以这种方式调用，将返回一个 `promise`。

```javascript
import('/modules/my-module.js').then((module) => {
    // Do something with the module.
});
```

这种使用方式也支持 `await` 关键字

```javascript
let module = await import('/modules/my-module.js');
```

<!--more-->



##### `eval()`

```javascript
jQuery.ajax('https://matrix.datastory.com.cn/frontApp/get?id=535&name=index.js').done(jsJson => {
    // js字符串代码中，将module作为返回值
	window.module = window.eval(jsJson);
	console.log(module);
});
```

```javascript
// jsJson
(function(t) {
    var r = {};
    function e(n) {
        if (r[n]) {
            return r[n].exports
        }
        var i = r[n] = {
            i: n,
            l: false,
            exports: {}
        };
        t[n].call(i.exports, i, i.exports, e);
        i.l = true;
        return i.exports
    }
    e.m = t;
    e.c = r;
    e.d = function(t, r, n) {
        if (!e.o(t, r)) {
            Object.defineProperty(t, r, {
                configurable: false,
                enumerable: true,
                get: n
            })
        }
    };
    e.n = function(t) {
        var r = t && t.__esModule ?
        function r() {
            return t["default"]
        }: function r() {
            return t
        };
        e.d(r, "a", r);
        return r
    };
    e.o = function(t, r) {
        return Object.prototype.hasOwnProperty.call(t, r)
    };
    e.p = "";
    return e(e.s = 10)
})([
    // ...
    // 10
    function(t, r, e) {
        function n(t) {
            e(11)
        }
        var i = e(21);
        var o = e(22);
        var a = e(52);
        var f = false;
        var u = n;
        var s = "data-v-60bfadd4";
        var c = null;
        var l = i(o, a, f, u, s, c);
        t.exports = l.exports
    },
    // ...
]);
```

