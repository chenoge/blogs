---
title: CSS-font-face
date: 2019-12-19 11:43:57
tags: [css,font-face]
---

#### @font-face语法

```
@font-face {
    font-family: <webFontName>;
    src: <source> [<format>][,<source> [<format>]]*;
    [font-weight: <weight>];
    [font-style: <style>];
}
```

注： `format`字体格式，用于帮助浏览器识别，`truetype opentype truetype-aat embedded-opentype svg` 

<!--more-->



#### `format`格式

1. `truetype - ttf `
   - Windows 和 Mac 最常见字体
   - RAW 格式，不为任何网站优化
   - `IE9+、Firefox3.5+、Chrome4+、Safari3+、Opera10+、iOS Mobile Safari4.2+`
2. `opentype - otf `
   - 原始字体格式，内置在 truetype 基础之上
   - 提供更多功能
   - `Firefox3.5+、Chrome4.0+、Safari3.1+、Opera10.0+、iOS Mobile Safari4.2+`
3. `web-open-font-format - woff `
   - Web 字体最佳格式
   - 是一个开放的 truetype、opentype 压缩版本
   - 支持元数据包的分离
   - `IE9+、Firefox3.5+、Chrome6+、Safari3.6+、Opera11.1+`
4. `embedded-opentype - eot` 
   - IE 专用字体
   - 可以从 truetype 创建此格式
   - IE4+
5. `svg - svg` 
   - 基于 svg 渲染
   - `Chrome4+、Safari3.1+、Opera10.0+、iOS Mobile Safari3.2+`



```css
@font-face {
    font-family: 'webFontName';
    src: url('webFontName.eot'); /* IE9 Compat Modes */
    src: url('webFontName.eot?#iefix') format('embedded-opentype'), /* IE6-IE8 */
    url('webFontName.woff2') format('woff2'),
    url('webFontName.woff') format('woff'), /* Modern Browsers */
    url('webFontName.ttf') format('truetype'), /* Safari, Android, iOS */
    url('webFontName.svg#webFontName') format('svg'); /* Legacy iOS */
}
```