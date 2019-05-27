---
title: javascript-sort
date: 2019-04-08 10:08:27
tags: [sort]
---

#### 一、语法

```
arr.sort([compareFunction])
```

如果没有指明 `compareFunction` ，元素会按照**转换为字符串**的诸个字符的`Unicode`位点进行排序。

如果指明 `compareFunction` ，数组会按照调用该函数的**返回值排序**。即 a 和 b 是两个将要被比较的元素：

- 如果 `compareFunction(a, b)` 小于 0 ，那么 a 会被排列到 b 之前

- 如果 `compareFunction(a, b)` 等于 0 ， a 和 b 的相对位置不变
  - 备注： ECMAScript 标准并不保证这一行为，而且也不是所有浏览器都会遵守

- 如果 `compareFunction(a, b)` 大于 0 ， a 会被排列到 b 之后

<br/>



#### 二、内部排序

**V8 引擎** sort 函数只给出了两种排序分别是： `InsertionSort` 和` QuickSort`

- 数组长度小于等于10的用插入排序 `InsertionSort`
- 比10大的数组则使用快速排序 `QuickSort `

<!--more-->

<br/>



#### 三、特定序列排序

```javascript
['d', 'a', 'b', '2', '1', '3'].sort((x, y) => {
    let orderList = ['b', 'a'];
    let xIndex = orderList.indexOf(x);
    let yIndex = orderList.indexOf(y);
    let res = xIndex - yIndex;
    
    if (xIndex === -1) {
        res = 1;
    }
    
    if (yIndex === -1) {
        res =  -1;
    }
    
    if (xIndex === -1 && yIndex === -1) {
        res = x > y ? -1 : 1;
    }

    console.log(x, y, res);
    
    return res;
});
```

<br/>



#### 四、[源码](https://github.com/v8/v8/blob/ad82a40509c5b5b4680d4299c8f08d6c6d31af3c/src/js/array.js)

```javascript
function InnerArraySort(array, length, comparefn) {
    // In-place QuickSort algorithm.
    // For short (length <= 22) arrays, insertion sort is used for efficiency.

    if (!IS_CALLABLE(comparefn)) {
        comparefn = function(x, y) {
            if (x === y) return 0;
            if ( % _IsSmi(x) && % _IsSmi(y)) {
                return %SmiLexicographicCompare(x, y);
            }
            x = TO_STRING(x);
            y = TO_STRING(y);
            if (x == y) return 0;
            else return x < y ? -1 : 1;
        };
    }

    var InsertionSort = function InsertionSort(a, from, to) {
        for (var i = from + 1; i < to; i++) {
            var element = a[i];
            for (var j = i - 1; j >= from; j--) {
                var tmp = a[j];
                var order = comparefn(tmp, element);
                if (order > 0) {
                    a[j + 1] = tmp;
                } else {
                    break;
                }
            }
            a[j + 1] = element;
        }
    };

    var GetThirdIndex = function(a, from, to) {
        var t_array = new InternalArray();
        // Use both 'from' and 'to' to determine the pivot candidates.
        var increment = 200 + ((to - from) & 15);
        var j = 0;
        from += 1;
        to -= 1;
        for (var i = from; i < to; i += increment) {
            t_array[j] = [i, a[i]];
            j++;
        }
        t_array.sort(function(a, b) {
            return comparefn(a[1], b[1]);
        });
        var third_index = t_array[t_array.length >> 1][0];
        return third_index;
    }

    var QuickSort = function QuickSort(a, from, to) {
        var third_index = 0;
        while (true) {
            // Insertion sort is faster for short arrays.
            if (to - from <= 10) {
                InsertionSort(a, from, to);
                return;
            }
            if (to - from > 1000) {
                third_index = GetThirdIndex(a, from, to);
            } else {
                third_index = from + ((to - from) >> 1);
            }
            // Find a pivot as the median of first, last and middle element.
            var v0 = a[from];
            var v1 = a[to - 1];
            var v2 = a[third_index];
            var c01 = comparefn(v0, v1);
            if (c01 > 0) {
                // v1 < v0, so swap them.
                var tmp = v0;
                v0 = v1;
                v1 = tmp;
            } // v0 <= v1.
            var c02 = comparefn(v0, v2);
            if (c02 >= 0) {
                // v2 <= v0 <= v1.
                var tmp = v0;
                v0 = v2;
                v2 = v1;
                v1 = tmp;
            } else {
                // v0 <= v1 && v0 < v2
                var c12 = comparefn(v1, v2);
                if (c12 > 0) {
                    // v0 <= v2 < v1
                    var tmp = v1;
                    v1 = v2;
                    v2 = tmp;
                }
            }
            // v0 <= v1 <= v2
            a[from] = v0;
            a[to - 1] = v2;
            var pivot = v1;
            var low_end = from + 1; // Upper bound of elements lower than pivot.
            var high_start = to - 1; // Lower bound of elements greater than pivot.
            a[third_index] = a[low_end];
            a[low_end] = pivot;

            // From low_end to i are elements equal to pivot.
            // From i to high_start are elements that haven't been compared yet.
            partition: for (var i = low_end + 1; i < high_start; i++) {
                var element = a[i];
                var order = comparefn(element, pivot);
                if (order < 0) {
                    a[i] = a[low_end];
                    a[low_end] = element;
                    low_end++;
                } else if (order > 0) {
                    do {
                        high_start--;
                        if (high_start == i) break partition;
                        var top_elem = a[high_start];
                        order = comparefn(top_elem, pivot);
                    } while (order > 0);
                    a[i] = a[high_start];
                    a[high_start] = element;
                    if (order < 0) {
                        element = a[i];
                        a[i] = a[low_end];
                        a[low_end] = element;
                        low_end++;
                    }
                }
            }
            if (to - high_start < low_end - from) {
                QuickSort(a, high_start, to);
                to = low_end;
            } else {
                QuickSort(a, from, low_end);
                from = high_start;
            }
        }
    };
```

