---
title: JS-检测字体
date: 2018-04-15 21:52:09
tags: [javaScript,字体]
---

# 一、实现原理

根据用户设置的字体将某一个字符绘制在canvas上（**fillText**()），并提取像素信息（**getImageData**()），然后**和默认字体进行比对**，<font color=#A52A2A size=4 >**如果像素不一致，说明字体生效，说明字体不生效**</font>。

```javascript
  let isSupportFontFamily = function(fontFamily) {
      if (typeof fontFamily !== 'string') {
          return false;
      }


      let defaultFontFamily = 'Arial';
      if (fontFamily.toLowerCase() === defaultFontFamily.toLowerCase()) {
          return true;
      }


      let defaultLetter = 'a';
      let defaultFontSize = 100;


      // 使用该字体绘制的canvas
      let width = 100;
      let height = 100;
      let canvas = document.createElement('canvas');
      let context = canvas.getContext('2d');
      canvas.width = width;
      canvas.height = height;


      // 全局一致的绘制设定
      context.textAlign = 'center';
      context.fillStyle = 'black';
      context.textBaseline = 'middle';


      let getFontData = function(fontFamily) {
          context.clearRect(0, 0, width, height);
          context.font = defaultFontSize + 'px ' + fontFamily + ', ' + defaultFontFamily;
          context.fillText(defaultLetter, width / 2, height / 2);
          let data = context.getImageData(0, 0, width, height).data;
          return [].slice.call(data).filter(function(value) {
              return value !== 0;
          });
      };

      return getFontData(defaultFontFamily).join('') !== getFontData(fontFamily).join('');
  };
```

