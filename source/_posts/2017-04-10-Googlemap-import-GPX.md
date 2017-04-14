---
title: Googlemap-import-GPX
comments: false
categories: [Coding, Android]
tags: [Android, Google map]
date: 2017-04-10 11:10:00
---

套件`android-gpx-parser`:
<https://github.com/ticofab/android-gpx-parser>
<http://ticofab.io/android-gpx-parsing/>

<br>
* * *

Gradle
=====
```
compile 'io.ticofab.androidgpxparser:parser:0.2.0'
```

<br>
* * *

Custom GPX Class
=====
```java
public class CustomGpx {
    private final static String TAG = "CustomGpx";
    private Gpx mGpx;

    public CustomGpx(Gpx gpx) {
        mGpx = gpx;
    }

    public List<WayPoint> getWayPoints() {
        return mGpx.getWayPoints();
    }

    public List<Route> getRoutes() {
        return mGpx.getRoutes();
    }

    public List<Track> getTracks() {
        return mGpx.getTracks();
    }

	// 將航跡畫在地圖上(只取trk0, seg0)
    public Polyline drawTrk(GoogleMap map) {
        List<TrackPoint> trkPts =
                this.getTracks().get(0).getTrackSegments().get(0).getTrackPoints();
        List<LatLng> latLngs = new ArrayList<LatLng>();

        for (TrackPoint trkPt : trkPts) {
            LatLng latLng = new LatLng(trkPt.getLatitude(), trkPt.getLongitude());
            latLngs.add(latLng);
        }

        return map.addPolyline(new PolylineOptions()
                .addAll(latLngs)
                .color(Color.RED)
                .startCap(new CustomCap(BitmapDescriptorFactory.fromResource(
                        R.drawable.ic_start_point_24dp), 10))
                .endCap(new CustomCap(BitmapDescriptorFactory.fromResource(
                        R.drawable.ic_arrowhead_white), 5))
                .jointType(JointType.ROUND)
                .zIndex(200));
    }

    // 讀入GPX檔
    public static CustomGpx parseGpx(InputStream in){
        GPXParser mParser = new GPXParser(); // consider injection
        try {
            return new CustomGpx(mParser.parse(in));
        } catch (IOException | XmlPullParserException e) {
            // do something with this exception
            e.printStackTrace();
            Log.i(TAG, "parseGpx: parsed failed");
        }
        return null;
    }
}
```

<br>
* * *

In Activity
=====
```java
// Test for GPX
CustomGpx gpx = CustomGpx.parseGpx(getResources().openRawResource(R.raw.test));
gpx.drawTrk(mMap);
```