---
title: Googlemap-code-stored
comments: false
categories: [Coding, Android]
tags: [Android, Google map]
date: 2017-04-04 19:57:11
---

求取位置授權
======
<br>
```java
//Get the permission to access users' location
if (checkPermission())
	mMap.setMyLocationEnabled(true);
else askPermission();
```
```java
// Check for permission to access Location
private boolean checkPermission() {
	Log.d(TAG, "checkPermission()");
	// Ask for permission if it wasn't granted yet
	return (ContextCompat.checkSelfPermission(this, Manifest.permission.ACCESS_FINE_LOCATION)
			== PackageManager.PERMISSION_GRANTED);
}

// Asks for permissions
private void askPermission() {
	Log.d(TAG, "askPermission()");
	ActivityCompat.requestPermissions(
			this,
			new String[]{Manifest.permission.ACCESS_FINE_LOCATION},
			PERMISSIONS_REQUEST_ACCESS_FINE_LOCATION
	);
}

// Reaction to whether user granted or denied permissions
@Override
public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
	Log.d(TAG, "onRequestPermissionsResult()");
	super.onRequestPermissionsResult(requestCode, permissions, grantResults);
	switch (requestCode) {
		case PERMISSIONS_REQUEST_ACCESS_FINE_LOCATION: {
			if (grantResults.length > 0
					&& grantResults[0] == PackageManager.PERMISSION_GRANTED) {
				// Permission granted
				if (checkPermission())
					mMap.setMyLocationEnabled(true);

			} else {
				// Permission denied

			}
			break;
		}
	}
}
```

<br>
* * *

Camera
======
<br>
控制縮放範圍
```java
// Set the boundaries of Taiwan
mMap.setLatLngBoundsForCameraTarget(TAITWN_BOUNDS);
mMap.setMinZoomPreference(7);
```

```java
mMap.moveCamera(CameraUpdateFactory.newLatLngZoom(taiwan, 15));
```

<br>
* * *

Marker
======
<br>
加入標記(waypt)，可自訂圖示
```java
// Add a marker in Sydney and move the camera
LatLng taiwan = new LatLng(23.811360, 121.454273);
Marker marker1 = mMap.addMarker(new MarkerOptions()
		.icon(BitmapDescriptorFactory.fromResource(R.drawable.ic_settings_remote_white_24dp))
		.anchor(0.0f, 1.0f) // Anchors the marker on the bottom left
		.position(taiwan)
		.title("Custom Marker"));
```

<br>
* * *

Listener
======
<br>
```java
// Test when camera stop

mZoom = mMap.getCameraPosition().zoom;

mMap.setOnCameraIdleListener(new GoogleMap.OnCameraIdleListener() {
	@Override
	public void onCameraIdle() {
		if (mMap.getCameraPosition().zoom != mZoom) {
			mZoom = mMap.getCameraPosition().zoom;
			Toast.makeText(MapsActivity.this, "" + mZoom, Toast.LENGTH_SHORT).show();
		}
	}
});
```

Use ClusterManager
```java
mMap.setOnMarkerClickListener(mClusterManager);
mMap.setOnCameraIdleListener(mClusterManager);
```

長按時增加標記，並加入ClusterManager
```java
// Test for long click
mMap.setOnMapLongClickListener(new GoogleMap.OnMapLongClickListener() {
	@Override
	public void onMapLongClick(LatLng latLng) {
		MyClusterItem newClusterItem = new MyClusterItem(latLng.latitude, latLng.longitude, "test pt", latLng.toString());
		mClusterManager.addItem(newClusterItem);
		// Let the marker show on map instantly.
		mClusterManager.cluster();
	}
});
```