---
title: Googlemap-GeoJSON
comments: false
categories: [Coding, Android]
tags: [Android, Google map]
date: 2017-04-05 10:59:47
---

[StackOverflow](http://stackoverflow.com/questions/35158075/google-maps-marker-clustering-with-large-10-000-geojson)

[Google Maps Android API utility library](http://googlemaps.github.io/android-maps-utils/javadoc/com/google/maps/android/geojson/GeoJsonFeature.html)


將res/raw中的JSON檔讀入cluster
=====

使用Google Maps Android API utility library

```java
//Test for GeoJSON
try {
	GeoJsonLayer layer = new GeoJsonLayer(mMap, R.raw.trkpt,
			getApplicationContext());
	for (GeoJsonFeature f:layer.getFeatures()){
		String name = f.getProperty("name");
		GeoJsonPoint coordinate = (GeoJsonPoint) f.getGeometry();
		LatLng latLng = coordinate.getCoordinates();
		// Throw infos into cluster as marker
		MyClusterItem newClusterItem = new MyClusterItem(latLng, name, latLng.toString());
		mClusterManager.addItem(newClusterItem);
		// Let the marker show on map instantly.
		mClusterManager.cluster();
	}

} catch (Exception e) {
	Log.i(TAG, "JSONError in processing JSON");
	e.printStackTrace();
}
```

使用套件cocoahero: <https://github.com/cocoahero/android-geojson>

```java
import com.cocoahero.android.geojson.Feature;
import com.cocoahero.android.geojson.FeatureCollection;
import com.cocoahero.android.geojson.GeoJSON;
```

```java
//Test for GeoJSON
try {
	// Read JSON file from res/raw
	InputStream inputStream = getResources().openRawResource(R.raw.trkpt);
	// Read stream based on JSON "type"
	FeatureCollection pts = (FeatureCollection) GeoJSON.parse(inputStream);
	for (Feature pt : pts.getFeatures()) {
		String name = pt.getProperties().getString("name");
		JSONArray coordinates = pt.getGeometry().toJSON().getJSONArray("coordinates");
		LatLng latLng = new LatLng(coordinates.getDouble(1), coordinates.getDouble(0));
		Log.i(TAG, name + " / " + latLng);
		// Throw infos into cluster as marker
		MyClusterItem newClusterItem = new MyClusterItem(latLng, name, latLng.toString());
		mClusterManager.addItem(newClusterItem);
		// Let the marker show on map instantly.
		mClusterManager.cluster();
	}
} catch (IOException e) {
	Log.i(TAG, "IOError in processing JSON");
	e.printStackTrace();
} catch (JSONException e) {
	Log.i(TAG, "JSONError in processing JSON");
	e.printStackTrace();
}
```

<br>
* * *
自訂PolyLine
=====

ref: <https://maps-apis.googleblog.com/2017/02/styling-and-custom-data-for-polylines.html>
sample: <https://github.com/googlemaps/android-samples>

```java
// Test for custom track
Polyline mTestTrack;
List<LatLng> mTestTrackPoints = new ArrayList<LatLng>();
List<PatternItem> dashedPattern = Arrays.asList(new Dash(50), new Gap(30));
```

```java
try {
	// 嵐山航跡
	GeoJsonLayer layer = new GeoJsonLayer(mMap, R.raw.trk, getApplicationContext());
	GeoJsonFeature trkFeature = layer.getFeatures().iterator().next();
	GeoJsonMultiLineString trk = (GeoJsonMultiLineString) trkFeature.getGeometry();
	GeoJsonLineString lineString = trk.getLineStrings().iterator().next();
	for (LatLng latLng : lineString.getCoordinates()) {
		mTestTrackPoints.add(latLng);
	}
	// 航跡style測試
	// joint-轉彎處/startCap-起點圖標/endCap-終點圖標/pattern-線段樣式(不能使用Bimap)
	mTestTrack = mMap.addPolyline(new PolylineOptions()
			.color(Color.BLUE)
			.pattern(dashedPattern)
			.startCap(new CustomCap(BitmapDescriptorFactory.fromResource(
					R.drawable.ic_start_point_24dp), 10))
			.endCap(new CustomCap(BitmapDescriptorFactory.fromResource(
					R.drawable.ic_arrowhead_white), 5))
			.jointType(JointType.ROUND)
			.zIndex(100));
	mTestTrack.setPoints(mTestTrackPoints);

} catch (Exception e) {
	Log.i(TAG, "Error in processing JSON file");
	e.printStackTrace();
}
```

<br>
* * *

其它參考資料
=====
<https://github.com/ProfPaulB/JSONinRAWExample>