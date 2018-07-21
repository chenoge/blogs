---
title: 点击取词-Selection和Range
date: 2018-07-21 18:56:35
tags: [Selection,Range,点击取词,划词]
---

[原文](http://www.fedlab.tech/archives/2420.html)

<br/>

# 一、对象

[Selection](https://developer.mozilla.org/en-US/docs/Web/API/Selection) 对象表示用户**选择的文本范围**或**插入符号的当前位置**。它代表页面中的文本选区，可能横跨多个元素。文本选区由用户拖拽鼠标经过文字而产生。要获取用于检查或修改的Selection对象，请调用 `window.getSelection()`。

<br/>

[Range](https://www.w3.org/TR/DOM-Level-2-Traversal-Range/ranges.html)，表示**包含节点**和**部分文本节点的文档片段**，可以通过 Selection 对象的 getRangeAt 方法取得，也可以通过 Document 对象的 createRange 方法创建。

<br/>

<!--more-->

# 二、应用

在我们日常前端开发中，可能会遇到这样的场景，实现**划词翻译**、**点击取词翻译**、**编辑器中的复制、粘贴等需求**，下面我们通过对这两种需求场景来介绍 Selection 对象 和 Range 对象在实际项目中的应用。

<br/>

#### 实现点击取词并翻译

实现基本思路，点击时获取 Selection 对象，并创建选取，通过 Range 的 setStart 和 setEnd 方法扩大文档片段的范围，以空格或者特殊符号为临界状态，终止循环。

##### 1.方法一实现相关逻辑如下：

```javascript
if (window.getSelection) {
    let selection, range, node, start, end, text, rect, isFun, isText;

    // 回调函数判断
    isFun = !!window.JSInvoker && typeof window.JSInvoker.toggleWordTooltip === 'function';

    // 文本判断
    isText = function (text) {
        return text && /^[a-zA-Z]+$/.test(text);
    }

    // 如果 tooltip 存在，需要先关闭
    isFun && window.JSInvoker.toggleWordTooltip(false);

    selection = window.getSelection();
    range = selection.getRangeAt(0);
    node = selection.anchorNode;
    start = range.startOffset;
    end = range.endOffset;
    text = range.toString();

    // <<<向前判断非空格边界
    while (!/^\s+/.test(text)) {
        if (start === 0) {
            range.setStart(node, start);
            text = range.toString();
            break;
        } else {
            start -= 1;
            range.setStart(node, start);
            text = range.toString();
        }
        if (!isText(text)) {
            start += 1;
            range.setStart(node, start);
            text = range.toString();
            break;
        }
    }
    
    // >>>向后判断非空格边界
    while (!/\s+$/.test(text)) {
        if (end === node.length) {
            range.setEnd(node, end);
            text = range.toString();
            break;
        } else {
            end += 1;
            range.setEnd(node, end);
            text = range.toString();
        }
        if (!isText(text)) {
            end -= 1;
            range.setEnd(node, end);
            text = range.toString();
            break;
        }
    }

    // 获取当前文档片段位置信息
    rect = range.getBoundingClientRect();
    text = text.trim();
    if (isText(text) && isFun && rect) {
        // 打开 tooltip
        window.JSInvoker.toggleWordTooltip(true, text, rect.x, rect.y, rect.width, rect.height);
    } else {
        alert(`Text: ${text}, isFun: ${isFun}, Rect: ${JSON.stringify(rect)}`);
    }
} else {
    // todo
}
```

<br/>

##### 2.方法二实现方案比较简单，实现代码逻辑如下：

```javascript
if (window.getSelection) {
    let selection, start, end, word;

    selection = window.getSelection();

    selection.modify('extend', 'backward', 'word');
    start = selection.toString();

    selection.modify('extend', 'forward', 'word');
    end = selection.toString();

    selection.modify('move', 'forward', 'character');

    word = start + end;
    alert(`Selected Text: ${word}`);
} else {
    // todo
}
```

<br/>

#### 实现划词翻译

##### 1.获取划词文本

```javascript
function getSelected () {
    let selection;
    if (window.getSelection) {
        // webkit and mozilla and IE9 +
        selection = window.getSelection();
    } else if (document.getSelection) {
        // 同上
        selection = document.getSelection();
    } else if (document.selection) {
        // IE
        selection = document.selection.createRange().text;
    }

    return selection.toString().trim();
}
```

##### 2.清空选中文本

```javascript
function removeSelection () {
    if (window.getSelection) {
        window.getSelection().removeAllRanges();
    } else if (document.getSelection && document.getSelection.empty) {
        document.getSelection().empty();
    } else if (document.selection && document.selection.empty) {
        document.selection.empty();
    }
}
```