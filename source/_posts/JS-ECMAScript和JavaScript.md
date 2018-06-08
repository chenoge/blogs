---
title: JS-ECMAScript和JavaScript
date: 2018-05-22 11:40:53
tags: [javaScript,ECMAScript]
---

由 **ECMA-262 定义的 ECMAScript 与 Web 浏览器没有依赖关系。实际上，这门语言本身<font color=#A52A2A size=4 >并不包含输 入和输出定义</font>。**ECMA-262 定义的只是这门语言的基础，而在此基础之上可以构建更完善的脚本语言。


ECMAScript规定了这 门语言的下列组成部分：

- 语法
- 类型 
- 语句 
- 关键字 
- 保留字 
- 操作符 
- 对象 

**ECMAScript 就是对实现该标准规定的各个方面内容的语言的<font color=#A52A2A size=4 >描述</font>**。JavaScript 实现了 ECMAScript， Adobe ActionScript 同样也实现了 ECMAScript。


我们常见的 Web 浏览器只是 ECMAScript 实现可能的宿主环境之一。宿主环境不仅提供基本的 ECMAScript 实现，同时也会提供该语言的扩展，以便语言与环境之间对接交互。而这些扩展——如 DOM，则利用 ECMAScript 的核心类型和语法提供更多更具体的功能，以便实现针对环境的操作。其他 宿主环境包括 Node（一种服务端 JavaScript 平台）和 Adobe Flash。


一个完整的 JavaScript 实现应该由下列三 个不同的部分组成

- 核心（ECMAScript） 
- 文档对象模型（DOM） 
- 浏览器对象模型（BOM）