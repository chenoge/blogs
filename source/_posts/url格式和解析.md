---
title: url格式和解析
date: 2019-02-12 14:22:28
tags: [url,解析,格式]
---

#### `url`格式

`node.js`的`url` 模块提供了两套 API 来处理 URL：一个是旧版本遗留的 API，一个是实现了 [WHATWG标准](http://nodejs.cn/s/fKgW8d)的新 API。在下图中，URL `'http://user:pass@sub.example.com:8080/p/a/t/h?query=string#hash'` **上方**的是遗留的 `url.parse()` 返回的对象的属性。 **下方**的则是` WHATWG` 的 `URL` 对象的属性。 

![url格式](url格式和解析\1.png)

<!--more-->

<br/>

#### url解析

```javascript
export const parseUrl = function (url) {
    let a = document.createElement('a');
    a.href = url;
    return {
        source: url,
        protocol: a.protocol.replace(':', ''),
        host: a.hostname,
        port: a.port,
        query: a.search,
        params: (function () {
            let ret = {},
                seg = a.search.replace(/^\?/, '').split('&'),
                len = seg.length,
                i = 0,
                s;
            for (; i < len; i++) {
                if (!seg[i]) {
                    continue;
                }
                s = seg[i].split('=');
                ret[s[0]] = s[1];
            }
            return ret;
        })(),
        file: (a.pathname.match(/\/([^\/?#]+)$/i) || [, ''])[1],
        hash: a.hash.replace('#', ''),
        path: a.pathname.replace(/^([^\/])/, '/$1'),
        relative: (a.href.match(/tps?:\/\/[^\/]+(.+)/) || [, ''])[1],
        segments: a.pathname.replace(/^\//, '').split('/')
    };
};
```