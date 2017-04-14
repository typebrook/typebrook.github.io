---
title: Googlemap-get-location
comments: false
categories: [Coding, Android]
tags: [Android, Google map]
date: 2017-04-07 17:56:23
---

[Android’s Location API  V.S.  Google’s Location Services API](http://stackoverflow.com/questions/33022662/android-locationmanager-vs-google-play-services)

目前強烈建議使用`Google Play services location APIs`
<http://blog.teamtreehouse.com/beginners-guide-location-android>

持續紀錄關鍵字: `WACK_LOCK` `PendingIntent`
<http://stackoverflow.com/questions/33311897/how-to-use-fusedlocation-api-to-monitor-location-updates-when-app-is-not-active?rq=1>

<br>
* * *

Method 1
=====
使用`Google Play services location APIs`

Google Doc: <https://developer.android.com/training/location/index.html>

中文範例: <http://www.codedata.com.tw/mobile/android-6-tutorial-4-3/>

make instance
=====

```java
// Test for track recording
List<LatLng> mMyTrackPoints = new ArrayList<LatLng>();
Polyline mMyTrack;
private Button mTracking;
boolean isTracking = false;
// Google API用戶端物件
GoogleApiClient mGoogleApiClient;
// 記錄目前最新的位置
Location mCurrentLocation;
LatLng mCurrentLatLng;
// Location請求物件
LocationRequest mLocationRequest;
LocationSettingsRequest.Builder builder = new LocationSettingsRequest.Builder()
		.addLocationRequest(mLocationRequest);
```

In `onCreate`
=====

```java
// Test for tracking
// Create an instance of GoogleAPIClient and LocationRequest.
configGoogleApiClient();
configLocationRequest();
PendingResult<LocationSettingsResult> result =
		LocationServices.SettingsApi.checkLocationSettings(mGoogleApiClient,
				builder.build());
```

```java
protected void configLocationRequest() {
	mLocationRequest = new LocationRequest();
	mLocationRequest.setInterval(10000);
	mLocationRequest.setFastestInterval(5000);
	mLocationRequest.setPriority(LocationRequest.PRIORITY_HIGH_ACCURACY);
}

private synchronized void configGoogleApiClient() {
	if (mGoogleApiClient == null) {
		mGoogleApiClient = new GoogleApiClient.Builder(this)
				.addConnectionCallbacks(this)
				.addOnConnectionFailedListener(this)
				.addApi(LocationServices.API)
				.build();
	}
}
```

implements `GoogleAPIClient`
=====

```java
implements GoogleApiClient.ConnectionCallbacks, GoogleApiClient.OnConnectionFailedListener
```

```java
// Test for Tracking
protected void onStart() {
	mGoogleApiClient.connect();
	super.onStart();
}

protected void onStop() {
	mGoogleApiClient.disconnect();
	super.onStop();
}

@Override
protected void onResume() {
	super.onResume();
	mGoogleApiClient.connect();
}

@Override
protected void onPause() {
	super.onPause();
	if (mGoogleApiClient.isConnected()) {
		LocationServices.FusedLocationApi.removeLocationUpdates(mGoogleApiClient, this);
		mGoogleApiClient.disconnect();
	}
}

// ConnectionCallbacks
// 已經連線到Google Services
@Override
public void onConnected(Bundle connectionHint) {
	if (checkPermission()) {
		// 啟動位置更新服務
		// 位置資訊更新的時候，應用程式會自動呼叫LocationListener.onLocationChange
		LocationServices.FusedLocationApi
				.requestLocationUpdates(mGoogleApiClient, mLocationRequest, this);
	}
}

// 或者自己選擇啟動
protected void startLocationUpdates() {
	if (checkPermission()) {
		LocationServices.FusedLocationApi.requestLocationUpdates(
				mGoogleApiClient, mLocationRequest, this);
	}
}

// Google Services連線中斷
@Override
public void onConnectionSuspended(int i) {
	// int參數是連線中斷的代號
}

// Google Services連線失敗
@Override
public void onConnectionFailed(@NonNull ConnectionResult connectionResult) {
	// ConnectionResult參數是連線失敗的資訊
	int errorCode = connectionResult.getErrorCode();

	// 裝置沒有安裝Google Play服務
	if (errorCode == ConnectionResult.SERVICE_MISSING) {
		Toast.makeText(this, R.string.google_play_service_missing,
				Toast.LENGTH_LONG).show();
	}
}
```

LocationListener(畫出航跡)
======

```java
implements LocationListener
```

```java
// LocationListener
@Override
public void onLocationChanged(Location location) {
	if (!isTracking){
		return;
	}
	Toast.makeText(this, location.toString(), Toast.LENGTH_LONG).show();
	mCurrentLocation = location;
	mCurrentLatLng = new LatLng(mCurrentLocation.getLatitude(), mCurrentLocation.getLongitude());
	mMyTrackPoints.add(mCurrentLatLng);
	mMyTrack.setPoints(mMyTrackPoints);

	// 移動地圖到目前的位置
	mMap.moveCamera(CameraUpdateFactory.newLatLng(mCurrentLatLng));
}
```