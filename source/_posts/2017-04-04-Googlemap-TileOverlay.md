---
title: Googlemap-TileOverlay
comments: false
categories: [Coding, Android]
tags: [Android, Google map]
date: 2017-04-04 19:34:25
---

Document: <https://developers.google.com/android/reference/com/google/android/gms/maps/model/TileOverlay>

[Code on GitHub](https://github.com/googlemaps/android-samples/blob/master/ApiDemos/app/src/main/java/com/example/mapdemo/TileOverlayDemoActivity.java)

```java    
// Test for WMTS tile 
private static final String SINICA_URL_FORMAT = "http://gis.sinica.edu.tw/tileserver/file-exists.php?img=TM25K_2001-jpg-%d-%d-%d";
```

```java
// Test for WMTS
TileProvider tileProvider = new UrlTileProvider(1024, 1024) {
	@Override
	public synchronized URL getTileUrl(int x, int y, int zoom) {

		String s = String.format(Locale.US, SINICA_URL_FORMAT, zoom, x, y);
		Log.i(TAG, "tile url: " + s);
		URL url = null;
		try {
			url = new URL(s);
		} catch (MalformedURLException e) {
			throw new AssertionError(e);
		}
		return url;
	}
};
TileOverlay mSinicaTiles = map.addTileOverlay(new TileOverlayOptions().tileProvider(tileProvider));
```