---
title: JS-replace
date: 2018-04-29 18:42:29
tags: [javaScript,replace]
---

# 一、语法

``` javascript
stringObject.replace(regexp/substr,replacement);
```

**在 stringObject 中查找与 regexp 相匹配的子字符串，然后用 replacement 来替换这些子串。**如果 regexp 具有<font color=#A52A2A size=4 >**全局标志 g**</font>，那么 replace() 方法将替换<font color=#A52A2A size=4 >所有</font>匹配的子串。否则，它只替换第一个匹配子串。

<br/>

<font color=#A52A2A size=4 >**replacement 可以是字符串，也可以是函数**</font>。如果它是字符串，那么每个匹配都将由字符串替换。

<br/>

replacement 中的 $ 字符具有特定的含义。如下表所示，它说明从模式匹配得到的字符串将用于替换。

| 字符             | 替换文本                                            |
| ---------------- | --------------------------------------------------- |
| $1、$2、...、$99 | 与 regexp 中的第 1 到第 99 个子表达式相匹配的文本。 |
| $&               | 与 regexp   相匹配的子串。                          |
| $`               | 位于匹配子串**左侧**的文本。                        |
| $'               | 位于匹配子串**右侧**的文本。                        |
| $$               | 直接量符号。                                        |

<br/>

# 二、replacement作为函数

replace() 方法的参数 replacement 可以是函数而不是字符串。在这种情况下，**每个匹配都调用该函数，它返回的字符串将作为替换文本使用**。该函数的第一个参数是匹配模式的字符串。接下来的参数是与模式中的子表达式匹配的字符串，可以有 0 个或多个这样的参数。接下来的参数是一个整数，声明了匹配在 stringObject 中出现的位置。最后一个参数是 stringObject 本身。

```javascript
name = "Doe, John";
name.replace(/(\w+)\s*, \s*(\w+)/, "$2 $1");

let _dete = '20180408000000'
function formatStr(str, type) {
    let i = 0,
        _type = type || "xxxx-xx-xx xx:xx:xx";
    return _type.replace(/x/g, () => str[i++]);
}

formatStr(_dete);
result: "2018-04-08 00:00:00"
```

