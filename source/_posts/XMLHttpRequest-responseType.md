---
title: XMLHttpRequest.responseType
date: 2019-07-31 11:54:49
tags: [XMLHttpRequest,responseType]
---

`XMLHttpRequest.responseType`属性是一个枚举类型的属性，返回响应数据的类型。它允许我们手动的设置返回数据的类型。如果我们将它设置为一个空字符串，它将使用默认的`text`类型。

<!--more-->

<br/>



| 值              | 描述                                                         |
| :-------------- | :----------------------------------------------------------- |
| `""`            | 将 `responseType` 设为空字符串与设置为`"text"`相同           |
| `"arraybuffer"` | [`response`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/response) 是一个包含二进制数据的 JavaScript [`ArrayBuffer`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/ArrayBuffer) 。 |
| `"blob"`        | `response` 是一个包含二进制数据的 [`Blob`](https://developer.mozilla.org/zh-CN/docs/Web/API/Blob) 对象 。 |
| `"document"`    | `response` 是一个 [HTML](https://developer.mozilla.org/en-US/docs/Glossary/HTML) [`Document`](https://developer.mozilla.org/zh-CN/docs/Web/API/Document) 或 [XML](https://developer.mozilla.org/en-US/docs/Glossary/XML) [`XMLDocument`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLDocument) ，这取决于接收到的数据的 MIME 类型。 |
| `"json"`        | `response` 是一个 JavaScript 对象。这个对象是通过将接收到的数据类型视为 [JSON](https://developer.mozilla.org/en-US/docs/Glossary/JSON) 解析得到的。 |
| `"text"`        | `response` 是包含在 [`DOMString`](https://developer.mozilla.org/zh-CN/docs/Web/API/DOMString) 对象中的文本。 |