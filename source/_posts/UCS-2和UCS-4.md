---
title: 字符编码与存储_UCS_UTF
date: 2018-12-07 20:32:19
tags: [UCS-2,UCS-4,UTF-8]
---

### 1、字节和字符的区别

`ASCII `时代，字节和字符是一样的。当`Unicode`出现后，事情有所不同了。`字节（octet）`是一个八位的**存储单元**，取值范围一定是`0～255`。而`字符（character，或者word）`为**语言意义上的符号**，范围就不一定了。例如在`UCS-2`中定义的字符范围为`0～65535`，它的一个字符占用两个字节。

<br/>

### 2、`UCS-2`和`UCS-4`

`Unicode`是为整合全世界的所有语言文字而诞生的。任何文字在`Unicode`中都对应一个值，这个值称为`代码点（code point）`。代码点的值通常写成` U+ABCD `的格式。而文字和代码点之间的对应关系就是`UCS-2（Universal Character Set coded in 2 octets）`。顾名思义，`UCS-2`是用两个字节来表示代码点，其取值范围为 `U+0000～U+FFFF`。

<br/>

为了能表示更多的文字，人们又提出了`UCS-4`，即用四个字节表示代码点。它的范围为 `U+00000000～U+7FFFFFFF`，其中 `U+00000000～U+0000FFFF`和`UCS-2`是一样的。

<br/>

要注意，`UCS-2`和`UCS-4`只规定了`代码点`和`文字`之间的对应关系，并没有规定代码点在计算机中如何存储。规定存储方式的称为`UTF（Unicode Transformation Format）`，其中应用较多的就是`UTF-8`。

<br/>

注：

- `Unicode`不是一次性定义的，而是**分区定义**。每个区可以存放`65536`个字符，称为一个平面（`plane`），定义了`17`个平面，目前`Unicode`字符集的大小是`1,114,112`。 



- 最前面的`65536`个字符位，称为**基本平面（缩写BMP）**，是`Unicode`最先定义和公布的一个平面。 

<br/>

<!--more-->

### 3、`UTF-8`

`UTF-8` 最大的一个特点，就是它是一种变长的编码方式。它可以使用`1~6`个字节表示一个符号，根据不同的符号而变化字节长度。

UTF-8 的编码规则很简单，只有二条：

- 对于单字节的符号，字节的第一位设为`0`，后面7位为这个符号的 Unicode 码。因此对于英语字母，UTF-8 编码和 ASCII 码是相同的。
- 对于`n`字节的符号（`n > 1`），第一个字节的前`n`位都设为`1`，第`n + 1`位设为`0`，后面字节的前两位一律设为`10`。剩下的没有提及的二进制位，全部为这个符号的 `Unicode` 码。

<br/>

下表总结了编码规则，字母`x`表示可用编码的位。

```markdown
Unicode符号范围      |        UTF-8编码方式
  (十六进制)         |         （二进制）
--------------------+-------------------------------------
0000 0000-0000 007F | 0xxxxxxx
0000 0080-0000 07FF | 110xxxxx 10xxxxxx
0000 0800-0000 FFFF | 1110xxxx 10xxxxxx 10xxxxxx
0001 0000-001F FFFF | 11110xxx 10xxxxxx 10xxxxxx 10xxxxxx
0020 0000-03FF FFFF | 111110xx 10xxxxxx 10xxxxxx 10xxxxxx 10xxxxxx
0400 0000-7FFF FFFF | 1111110x 10xxxxxx 10xxxxxx 10xxxxxx 10xxxxxx 10xxxxxx
```

注：

- 如果一个字节的第一位是`0`，则这个字节单独就是一个字符；如果第一位是`1`，则**连续有多少个`1`，就表示当前字符占用多少个字节**。
- 2003年11月`UTF-8`被`RFC 3629`重新规范，只能使用原来`Unicode`定义的区域，`U+0000`到`U+10FFFF`，也就是最多四个字节，`非标准UTF-8`仍支持使用`1~6`个字节表示一个符号。

<br/>

`严`的 Unicode 是`4E25`（`100111000100101`），根据上表，可以发现`4E25`处在第三行的范围内（`0000 0800 - 0000 FFFF`），因此`严`的 UTF-8 编码需要三个字节，即格式是`1110xxxx 10xxxxxx 10xxxxxx`。然后，从`严`的最后一个二进制位开始，依次从后向前填入格式中的`x`，多出的位补`0`。这样就得到了，`严`的 UTF-8 编码是`11100100 10111000 10100101`，转换成十六进制就是`E4B8A5`。

<br/>

**常用**中文**编码范围**（并非全部）

```
/[\u4e00-\u9fa5]/ 中文：20902个

/[\u0800-\u4e00]/ 日文：17921个

/[\uac00-\ud7ff]/ 韩文：11264个
```

注：以上正则表达式是**部分中文匹配**，并非精确，但能满足大多数情况。

<br/>

### 4、中文Unicode 编码范围

| **字符集**                                                   | **字数** | **Unicode 编码** |
| ------------------------------------------------------------ | -------- | ---------------- |
| [基本汉字](http://www.qqxiuzi.cn/zh/hanzi-unicode-bianma.php?zfj=jbhz) | 20902字  | 4E00-9FA5        |
| [基本汉字补充](http://www.qqxiuzi.cn/zh/hanzi-unicode-bianma.php?zfj=jbhzbc) | 38字     | 9FA6-9FCB        |
| [扩展A](http://www.qqxiuzi.cn/zh/hanzi-unicode-bianma.php?zfj=kza) | 6582字   | 3400-4DB5        |
| [扩展B](http://www.qqxiuzi.cn/zh/hanzi-unicode-bianma.php?zfj=kzb) | 42711字  | 20000-2A6D6      |
| [扩展C](http://www.qqxiuzi.cn/zh/hanzi-unicode-bianma.php?zfj=kzc) | 4149字   | 2A700-2B734      |
| [扩展D](http://www.qqxiuzi.cn/zh/hanzi-unicode-bianma.php?zfj=kzd) | 222字    | 2B740-2B81D      |
| [康熙部首](http://www.qqxiuzi.cn/zh/hanzi-unicode-bianma.php?zfj=kxbs) | 214字    | 2F00-2FD5        |
| [部首扩展](http://www.qqxiuzi.cn/zh/hanzi-unicode-bianma.php?zfj=bskz) | 115字    | 2E80-2EF3        |
| [兼容汉字](http://www.qqxiuzi.cn/zh/hanzi-unicode-bianma.php?zfj=jrhz) | 477字    | F900-FAD9        |
| [兼容扩展](http://www.qqxiuzi.cn/zh/hanzi-unicode-bianma.php?zfj=jrkz) | 542字    | 2F800-2FA1D      |
| [PUA(GBK)部件](http://www.qqxiuzi.cn/zh/hanzi-unicode-bianma.php?zfj=puabj) | 81字     | E815-E86F        |
| [部件扩展](http://www.qqxiuzi.cn/zh/hanzi-unicode-bianma.php?zfj=bjkz) | 452字    | E400-E5E8        |
| [PUA增补](http://www.qqxiuzi.cn/zh/hanzi-unicode-bianma.php?zfj=puazb) | 207字    | E600-E6CF        |
| [汉字笔画](http://www.qqxiuzi.cn/zh/hanzi-unicode-bianma.php?zfj=hzbh) | 36字     | 31C0-31E3        |
| [汉字结构](http://www.qqxiuzi.cn/zh/hanzi-unicode-bianma.php?zfj=hzjg) | 12字     | 2FF0-2FFB        |
| [汉语注音](http://www.qqxiuzi.cn/zh/hanzi-unicode-bianma.php?zfj=hyzy) | 22字     | 3105-3120        |
| [注音扩展](http://www.qqxiuzi.cn/zh/hanzi-unicode-bianma.php?zfj=zykz) | 22字     | 31A0-31BA        |
| 〇                                                           | 1字      | 3007             |

<br/>

### 5、`UTF-16`

`UTF-16`的编码规则很简单：**基本平面**的字符占用2个字节，辅助平面的字符占用4个字节。**也就是说，UTF-16的编码长度要么是2个字节（U+0000到U+FFFF），要么是4个字节（U+010000到U+10FFFF）。**`UTF-16`编码是`UCS-2`的超集，介于`UTF-32`与`UTF-8`之间，同时结合了定长和变长两种编码方法的特点。

| Unicode 编号范围 （十六进制） | 0000 0000 ~ 0000 FFFF | 0001 0000 ~ 0010 FFFF                  |
| ----------------------------- | --------------------- | -------------------------------------- |
| Unicode 编号 （二进制）       | xxxx-xxxx ~ xxxx-xxxx | xxxx xxxx xxxx xxxx xxxx               |
| UTF-16 编码                   | xxxx-xxxx ~ xxxx-xxxx | 110110xx xxxx xxxx 110111xx  xxxx xxxx |
| 字节数                        | 2                     | 4                                      |

四字节存储：将字符编号的所有**比特位**分成两部分，较高的一些比特位用一个值介于 `D800~DBFF` 之间的双字节存储，较低的一些比特位用一个值介于` DC00~DFFF` 之间的双字节存储。 

注：标准的`UTF-8`和`UTF-16`都是最多支持`U+0000`到`U+10FFFF`的`Unicode `码点

<br/>

### 6、Unicode与JavaScript

`JavaScript`语言采用`Unicode`字符集，但是只支持一种编码方法。这种编码既不是`UTF-16`，也不是`UTF-8`，更不是`UTF-32`。**JavaScript用的是`UCS-2`**。

注：在 level 3 或者更高等级的实现中，遵循国际标准，` JavaScript 引擎`是允许使用 `UCS-2 `或者 `UTF-16` 进行编码的。 

<br/>

`JavaScript` 允许采用`\uxxxx`形式表示一个字符，其中`xxxx`表示字符的 `Unicode` 码点。 但是，这种表示法只限于码点在`\u0000`~`\uFFFF`之间的字符。超出这个范围的字符，必须用两个双字节的形式表示。 `ES6` 对这一点做出了改进，只要将码点放入大括号，就能正确解读该字符。

```javascript
"\u20BB7"
// "₻7" JavaScript会理解成\u20BB+7

"\u{20BB7}"
// "𠮷"

"\u{41}\u{42}\u{43}"
// "ABC"

let hello = 123;
hell\u{6F} // 123

'\u{1F680}' === '\uD83D\uDE80'
// true
```

<br/>

JavaScript 内部，字符以 `UTF-16` 的格式储存，每个字符固定为`2`个字节。 使用`for...of`循环，它会正确识别 32 位的 UTF-16 字符

```javascript
let s = '𠮷a';
for (let ch of s) {
  console.log(ch.codePointAt(0).toString(16));
}
// 20bb7
// 61
```

<br/>

`codePointAt`方法是测试一个字符由两个字节还是由四个字节组成的最简单方法

```javascript
function is32Bit(c) {
  return c.codePointAt(0) > 0xFFFF;
}

is32Bit("𠮷") // true
is32Bit("a") // false
```

<br/>
