---
title: JS-加载模块
date: 2018-04-29 17:11:41
tags: [javaScript,module]
---

```javascript
//防止多文件集成成一个文件后 前面的文件忘记写；的情况
;(function(factory) {
      var registeredInModuleLoader;
      if (typeof define === 'function' && define.amd) {
          console.log('a')
          define(factory);
          registeredInModuleLoader = true;
      }

      if (typeof exports === 'object') {
          console.log('b')
          module.exports = factory();
          registeredInModuleLoader = true;
      }

      if (!registeredInModuleLoader) {
          console.log('c')
          var OldCookies = window.Cookies;
          var api = window.Cookies = factory();
          api.noConflict = function() {
              window.Cookies = OldCookies;
              return api;
          };
      }
  }(function() { return { a: 1 } }));
```

