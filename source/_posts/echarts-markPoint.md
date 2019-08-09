---
title: echarts-markPoint
date: 2019-08-09 22:35:10
tags: [echarts,markPoint,markLine,markArea]
---

##### 整体说明

标注（markPoint）的数据数组。每个数组项是一个对象，有下面几种方式指定标注的位置：

1. 通过` x, y`属性指定相对容器的屏幕坐标，单位像素，支持百分比。
2. 用` coord` 属性指定数据在相应坐标系上的坐标位置，单个维度支持设置 `'min'`, `'max'`, `'average'`。
3. 直接用 `type`属性标注系列中的最大值，最小值。这时候可以使用 [valueIndex](https://echarts.baidu.com/option.html#series-bar.markPoint.data.valueIndex) 指定是在哪个维度上的最大值、最小值、平均值。或者可以使用 [valueDim](https://echarts.baidu.com/option.html#series-bar.markPoint.data.valueDim) 指定在哪个维度上的最大值、最小值、平均值。
4. 如果是笛卡尔坐标系的话，也可以通过只指定 `xAxis` 或者 `yAxis` 来实现 X 轴或者 Y 轴为某值的标线。

当多个属性同时存在时，优先级按上述的顺序。

<!--more-->

<br/>



##### markPoint.data

```javascript
data: [
    {
        name: '最大值',
        type: 'max'
    }, 
    {
        name: '某个坐标',
        coord: [10, 20]
    }, {
        name: '固定 x 像素位置',
        yAxis: 10,
        x: '90%'
    }, 
    {
        name: '某个屏幕坐标',
        x: 100,
        y: 100
    }
]
```

<br/>



##### markLine.data

```javascript
data: [
    {
        name: '平均线',
        // 支持 'average', 'min', 'max'
        type: 'average'
    },
    {
        name: 'Y 轴值为 100 的水平线',
        yAxis: 100
    },
    [
        {
            // 起点和终点的项会共用一个 name
            name: '最小值到最大值',
            type: 'min'
        },
        {
            type: 'max'
        }
    ],
    [
        {
            name: '两个坐标之间的标线',
            coord: [10, 20]
        },
        {
            coord: [20, 30]
        }
    ], 
    [
        {
            // 固定起点的 x 像素位置，用于模拟一条指向最大值的水平线
            yAxis: 'max',
            x: '90%'
        }, 
        {
            type: 'max'
        }
    ],
    [
        {
            name: '两个屏幕坐标之间的标线',
            x: 100,
            y: 100
        },
        {
            x: 500,
            y: 200
        }
    ]
]
```

<br/>



##### markArea.data

```javascript
data: [
    [
        {
            name: '平均值到最大值',
            type: 'average'
        },
        {
            type: 'max'
        }
    ],
    [
        {
            name: '两个坐标之间的标域',
            coord: [10, 20]
        },
        {
            coord: [20, 30]
        }
    ],
    [
        {
            name: '60分到80分',
            yAxis: 60
        },
        {
            yAxis: 80
        }
    ], 
    [
        {
            name: '所有数据范围区间',
            coord: ['min', 'min']
        },
        {
            coord: ['max', 'max']
        }
    ],
    [
        {
            name: '两个屏幕坐标之间的标域',
            x: 100,
            y: 100
        }, 
        {
            x: '90%',
            y: '10%'
        }
    ]
]
```

