---
title: JS-标签语句
date: 2018-05-07 16:23:25
tags: [javaScript,标签]
---

``` javascript
//label: statement;

outPoint:
    if (true) {
        for (var j = 0; j < all.length; j++) {
            for (var k = 0; k < all[j].length; k++) {
                if (all[j][k].id == itemsId[i]) {
                    console.log(all[j][k]);
                    break outPoint; //直接跳出最外层循环
                }
            }
        }
    }
```



- **JavaScript中任何地方都可以定义语句标签** 
- **break**和**continue**是JavaScript中唯一可以使用语句标签的语句 
- **控制权无法越过函数的边界**