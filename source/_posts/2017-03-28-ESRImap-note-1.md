---
title: ESRImap-筆記
comments: false
categories: [Coding, Android Studio, ESRI]
tags: [Android Studio, ESRI, Arcgis]
date: 2017-03-28 09:55:03
---

**使用中研院WMTS服務**
======

<br>
參考資料: <https://geonet.esri.com/thread/179250>
```
mMapView.setOnStatusChangedListener(new OnStatusChangedListener() {
	@Override
	public void onStatusChanged(Object o, OnStatusChangedListener.STATUS status) {
		if (status == STATUS.INITIALIZED) {
			AsyncTask.execute(new Runnable() {
				@Override
				public void run() {
					WMTSServiceInfo serviceInfo;
					try {
						serviceInfo =
								WMTSServiceInfo.fetch("http://gis.sinica.edu.tw/tileserver/wmts/1.0.0/WMTSCapabilities.xml");
						Log.i("serviceInfo", serviceInfo.toString());
						int count = 1;
						for (WMTSLayerInfo layerInfo : serviceInfo.getLayerInfos()){
							Log.i("No. " + count, layerInfo.getIdentifier());
							count++;
						}
						WMTSLayerInfo layerInfo = serviceInfo.getLayerInfos().get(28);
						final WMTSLayer layer = new WMTSLayer(layerInfo, SpatialReference.create(3857));
						layer.layerInitialise();
						layer.setOnStatusChangedListener(new OnStatusChangedListener() {
							@Override
							public void onStatusChanged(Object o, STATUS status) {
								if (status == STATUS.INITIALIZED) {
									mMapView.addLayer(layer);
								}
							}
						});
					} catch (Exception e) {
						Toast.makeText(context, "fetch fail", Toast.LENGTH_LONG).show();
						Log.i("WMTSServiceInfo", "fetch fail");
					}
				}
			});
		}
	}
});
```

<br>
* * *

**圖層順序**
======

```
No. 1: JM300K_1939
No. 2: JM20K_1904
No. 3: TM50K_2002
No. 4: JM50K_1924
No. 5: JM50K_1916
No. 6: TM25K_1989
No. 7: Admin_1930c
No. 8: TM50K_1956
No. 9: JM100K_1905
No. 10: JM500K_1936
No. 11: JM200K_1932
No. 12: Admin_1901c
No. 13: Taipei_aerialphoto_1945
No. 14: JM200K_1897
No. 15: Admin_1930a
No. 16: JM300K_1924
No. 17: Admin_1901b
No. 18: JM400K_1899
No. 19: TM100K_1987
No. 20: Admin_1901a
No. 21: JM25K_1921
No. 22: TM25K_2001
No. 23: TM50K_1996
No. 24: JM50K_1920
No. 25: AMCityPlan_1945
No. 26: JM20K_1904_Redraw
No. 27: JM25K_1942
No. 28: JM20K_1921
No. 29: JM25K_1944
No. 30: 1956_Landuse
No. 31: TM25K_1993
No. 32: TM50K_1990
No. 33: TM25K_2003
No. 34: Admin_1930b
No. 35: AM50K_1944
```