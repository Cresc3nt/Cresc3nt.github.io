---
layout: post
title: 带有GeoJSON的文章
date: 2024-01-26 17:57:00
description: 这是包含GeoJSON代码的样子
tags: [格式化, 图表, 地图]
categories: [示例文章]
map: true  # 启用地图功能
---

这是一篇包含一些[GeoJSON](https://geojson.org/)代码的示例文章。支持功能由[Leaflet](https://leafletjs.com/)提供。要创建您自己的可视化，请访问[geojson.io](https://geojson.io/)。

````markdown
```geojson
{
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "properties": {},
      "geometry": {
        "coordinates": [
          [
            [
              -60.11363029935569,
              -2.904625022183211
            ],
            [
              -60.11363029935569,
              -3.162613728707967
            ],
            [
              -59.820894493858034,
              -3.162613728707967
            ],
            [
              -59.820894493858034,
              -2.904625022183211
            ],
            [
              -60.11363029935569,
              -2.904625022183211
            ]
          ]
        ],
        "type": "Polygon"
      }
    }
  ]
}
```
````

渲染结果如下：

```geojson
{
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "properties": {},
      "geometry": {
        "coordinates": [
          [
            [
              -60.11363029935569,
              -2.904625022183211
            ],
            [
              -60.11363029935569,
              -3.162613728707967
            ],
            [
              -59.820894493858034,
              -3.162613728707967
            ],
            [
              -59.820894493858034,
              -2.904625022183211
            ],
            [
              -60.11363029935569,
              -2.904625022183211
            ]
          ]
        ],
        "type": "Polygon"
      }
    }
  ]
}
```
该 GeoJSON 数据定义了一个多边形区域，通常用于在地图上高亮显示特定地理范围。借助 al-folio 主题内置的 Leaflet 支持，此类 GeoJSON 内容可自动渲染为交互式地图。
