---
title: Googlemap-Z-Index
comments: false
categories: [Coding, Android]
tags: [Android, Google map]
date: 2017-04-04 16:35:48
---

Z-Index可使Google map的圖層能夠在堆疊時有次序的呈現，
數字大的圖層會疊在數字小的圖層之上。
包含`GroundOverlay`, `TileOverlay`, `Circle`, `Polyline`, `Polygon`, `Marker`
等物件都可以使用setZIndex(float z)來進行設定(或在initial時設定)。

每個圖層的預設值都是0，其值可設定小於0(用於TileOverlay)。

* * *

https://developers.google.com/maps/documentation/android-api/shapes#z-index

https://developers.google.com/android/reference/com/google/android/gms/maps/model/TileOverlay

* * *

sample
```java
private TileOverlay mSinicaTiles = map.addTileOverlay(new TileOverlayOptions().tileProvider(tileProvider));
mSinicaTiles.setZIndex(-10);
marker1.setZIndex(2);
```

