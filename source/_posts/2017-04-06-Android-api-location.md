---
title: 手機定位
comments: false
categories: [Coding, Android]
tags: [Android, GPS]
date: 2017-04-06 10:27:15
---

ref: <https://developer.android.com/guide/topics/sensors/index.html>

1. 有兩種方法: GPS 和 Android's Network Location Provider

2. Precise: GPS > Network
   GPS只能在室外運作
   耗電:    GPS較大
   速度: Network > GPS
   
3. 誤差因素:
   *   資訊來源大多(GPS, Cell-ID, and Wi-Fi)
   *   使用者經常在移動
   *   在不同位置，資訊來源的準確度也會不同
   
4. 使用`LocationManager`取得目前位置
   [Sample Code](https://developer.android.com/guide/topics/location/strategies.html#Updates)
   
5. `ACCESS_COARSE_LOCATION`只取得`NETWORK_PROVIDER`而已
   `ACCESS_FINE_LOCATION`包含有`NETWORK_PROVIDER`和`GPS_PROVIDER`
