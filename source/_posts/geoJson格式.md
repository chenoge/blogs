---
title: geoJson格式说明
date: 2019-07-18 15:09:51
tags: [geoJson,echarts]
---

#### GeoJSON

GeoJSON是一种对各种**地理数据结构**进行编码的格式，GeoJSON对象可以表示**几何、特征或者特征集合**。

GeoJSON支持下面几何类型：**点、线、面、多点、多线、多面和几何集合**。

GeoJSON里的**特征包含一个几何对象和其他属性**，特征集合表示一系列特征。

<!--more-->

<br/>



注：

- GeoJSON对象可能有**任何数目成员（名/值对**）

- GeoJSON对象必须由一个名字为`type`的成员，值必须是下面之一：

  ```
  Point, MultiPoint, LineString, MultiLineString, Polygon, MultiPolygon,
  GeometryCollection, Feature, FeatureCollection
  ```

- GeoJSON对象可能有一个可选的`crs`成员，它的值必须是一个坐标参考系统的对象
- GeoJSON对象可能有一个`bbox`成员，它的值必须是边界框数组

<br/>



```json
{
  "type": "FeatureCollection",
  "features": [
    // 点
    {
      "type": "Feature",
      "geometry": {"type": "Point", "coordinates": [102.0, 0.5]},
      "properties": {"prop0": "value0"}
    },
    // 线
    {
      "type": "Feature",
      "geometry": {
        "type": "LineString",
        "coordinates": [[102.0, 0.0], [103.0, 1.0], [104.0, 0.0], [105.0, 1.0]]
      },
      "properties": {
        "prop0": "value0",
        "prop1": 0.0
      }
    },
    // 面
    {
      "type": "Feature",
       "geometry": {
         "type": "Polygon",
         "coordinates": [
           [ [100.0, 0.0], [101.0, 0.0], [101.0, 1.0],
             [100.0, 1.0], [100.0, 0.0] ]
           ]
       },
       "properties": {
         "prop0": "value0",
         "prop1": {"this": "that"}
       }
    }
  ]
}
```

<br/>



传送门：[geojson在线效果](http://geojson.io/)