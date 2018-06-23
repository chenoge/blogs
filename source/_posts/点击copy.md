---
title: 点击copy
date: 2018-03-05 21:32:18
tags: [copy]
---

```html
<input type="text" id="inputText" value="测试文本" />
<input type="button" id="btn" value="复制" />
<textarea rows="4"></textarea>
```

```javascript
var btn = document.getElementById('btn');
btn.addEventListener('click', function() {
    var inputText = document.getElementById('inputText');
    var currentFocus = document.activeElement;

    inputText.focus();
    inputText.setSelectionRange(0, inputText.value.length);
    document.execCommand('copy', true);

    currentFocus.focus();
});
```

<br/>

<!--more--> 

```javascript
document.addEventListener('copy', function(e) {

    e.clipboardData.setData('text/plain', 'Hello, world!');

    e.clipboardData.setData('text/html', '<b>Hello, world!</b>');

    e.preventDefault();

});
```

<br/>

```javascript
const btn = document.querySelector('#btn');

btn.addEventListener('click', () => {
    const input = document.createElement('input');  
    input.setAttribute('readonly', 'readonly');
    input.setAttribute('value', 'hello world');

    document.body.appendChild(input);  
    input.setSelectionRange(0, 9999);

    if (document.execCommand('copy')) {    
        document.execCommand('copy');  
    }
    
    document.body.removeChild(input);
});
```

