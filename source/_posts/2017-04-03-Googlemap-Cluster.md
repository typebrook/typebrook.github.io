---
title: Googlemap-Cluster
comments: false
categories: [Coding, Android]
tags: [Android, Google map]
date: 2017-04-03 20:33:48
---

**教學資源**
======

<br>
1. [說明文件](https://developers.google.com/maps/documentation/android-api/utility/marker-clustering?hl=zh-tw)

2. 示範專案(需在gradle中將SDK升至目前版本):
```
git clone https://github.com/googlemaps/android-maps-utils.git
```

3. [Android API Utility Library](https://developers.google.com/maps/documentation/android-api/utility/)

* onClustersChanged - 調整camera時一定觸發，最先發生
  onClusterRendered - 有Cluster產生
  onBeforeClusterItemRendered - 有marker在畫面中


<br>
* * *

**Sample Code**
======

<br>
In Activity
```java
private ClusterManager<MyClusterItem> mClusterManager;
```

In custom class, MyClusterItem.java
```java
public class MyClusterItem implements ClusterItem {
    private final LatLng mPosition;
    private String mTitle;
    private String mSnippet;


    public MyClusterItem(double lat, double lng) {
        mPosition = new LatLng(lat, lng);
    }

    public MyClusterItem(double lat, double lng, String title, String snippet) {
        mPosition = new LatLng(lat, lng);
        mTitle = title;
        mSnippet = snippet;
    }

    @Override
    public LatLng getPosition() {
        return mPosition;
    }

    @Override
    public String getTitle() {
        return mTitle;
    }

    @Override
    public String getSnippet() {
        return mSnippet;
    }
}
```

In onMapReady()
```java
mClusterManager = new ClusterManager<>(this, mMap);
mMap.setOnMarkerClickListener(mClusterManager);
mMap.setOnCameraIdleListener(mClusterManager);
```

In onMapLongClick(), add a new marker into ClusterManager
```java
mMap.setOnMapLongClickListener(new GoogleMap.OnMapLongClickListener(){
	@Override
	public void onMapLongClick(LatLng latLng) {

		MyClusterItem newClusterItem = new MyClusterItem(latLng.latitude, latLng.longitude, "test pt", latLng.toString());
		mClusterManager.addItem(newClusterItem);
		// Let the marker show on map instantly.
		mClusterManager.cluster();
	}
});
```

<br>
* * *

Custom Renderer(Let the cluster item be draggable)
=====
<br>
*   refs

    [Stack Overflow](http://stackoverflow.com/questions/28773524/google-maps-android-marker-clustering-utility-cluster-items-not-being-removed)

    https://github.com/googlemaps/android-maps-utils.git -> CustomMarkerClusteringDemoActivity

new Renderer class
```java
import com.google.android.gms.maps.GoogleMap;
import com.google.android.gms.maps.model.MarkerOptions;
import com.google.maps.android.clustering.ClusterManager;
import com.google.maps.android.clustering.view.DefaultClusterRenderer;

/**
 * Created by pham on 2017/4/5.
 */

class NewRenderer extends DefaultClusterRenderer<MyClusterItem> {

    public NewRenderer(Context mContext, GoogleMap mMap, ClusterManager mClusterManager) {
        super(mContext, mMap, mClusterManager);
    }

    @Override
    protected void onBeforeClusterItemRendered(MyClusterItem item, MarkerOptions markerOptions) {
        markerOptions.draggable(true);
    }
}
```

In Activity
```java
mClusterManager.setRenderer(new NewRenderer(this, mMap, mClusterManager));
```

<br>
* * *

Click Event
=====
<br>

以`onClusterClick`為例，還有`onClusterItemClick`/`onClusterInfoWindowClick`/`onClusterItemInfoWindowClick`

```java
SomeClass implements ClusterManager.OnClusterClickListener<MyClusterItem>
```

```java
mClusterManager.setOnClusterClickListener(this);
```

點擊後，縮放到Cluster所有物件的邊界
```java
@Override
public boolean onClusterClick(Cluster<MyClusterItem> cluster) {
	// Zoom in the cluster. Need to create LatLngBounds and including all the cluster items
	// inside of bounds, then animate to center of the bounds.
	// Create the builder to collect all essential cluster items for the bounds.
	LatLngBounds.Builder builder = LatLngBounds.builder();
	for (MyClusterItem item : cluster.getItems()) {
		builder.include(item.getPosition());
	}
	// Get the LatLngBounds
	final LatLngBounds bounds = builder.build();

	// Animate camera to the bounds
	try {
		mMap.animateCamera(CameraUpdateFactory.newLatLngBounds(bounds, 100));
	} catch (Exception e) {
		e.printStackTrace();
	}

	return true;
}
```

<br>
* * *

Custom Icon
=====
<br>

Use [IconGenerator](https://github.com/googlemaps/android-maps-utils/blob/master/library/src/com/google/maps/android/ui/IconGenerator.java)

ref [StackOverflow](http://stackoverflow.com/questions/20791299/google-maps-utils-icongenerator-text-styles-and-background)


```java
private final IconGenerator mIconGenerator;
private TextView mTextView;
```

注意，icon來源可使用9-patch，在縮放時才不會影響文字
*   ref: 
        [Google Android Doc](https://developer.android.com/studio/write/draw9patch.html)
        [其它](http://www.cnblogs.com/Amandaliu/archive/2013/04/26/3045286.html)
		 
		 
```java
// test for custom icon
mIconGenerator = new IconGenerator(context);
mTextView = new TextView(context);
mTextView.setGravity(Gravity.CENTER);
mTextView.setTextColor(mContext.getResources().getColor(R.color.colorWayPoint));
mIconGenerator.setContentView(mTextView);
mIconGenerator.setBackground(mContext.getResources().getDrawable(R.drawable.ic_waypoint));
```


```java
// test for custom icon
@Override
protected void onBeforeClusterItemRendered(MyClusterItem item, MarkerOptions markerOptions) {

	mTextView.setText(item.getTitle());

	// Set the info window to show their name.
	Bitmap icon = mIconGenerator.makeIcon();
	markerOptions.icon(BitmapDescriptorFactory.fromBitmap(icon)).title(item.getTitle());
}
```