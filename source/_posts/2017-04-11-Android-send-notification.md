---
title: Android-發出系統通知
comments: false
categories: [Coding, Android]
tags: [Android, Google map]
date: 2017-04-11 12:01:06
---

ref: <http://www.codedata.com.tw/mobile/android-6-tutorial-5-2/>

```java
// 發出通知
private void sendNotify(Context context, long id) {
	// 取得NotificationManager物件
	NotificationManager manager = (NotificationManager) getSystemService(context.NOTIFICATION_SERVICE);
	// 建立NotificationCompat.Builder物件
	NotificationCompat.Builder builder =
			new NotificationCompat.Builder(context);
	// 設定小圖示、大圖示、狀態列文字、時間、內容標題、內容訊息和內容額外資訊
	builder.setSmallIcon(R.drawable.ic_double_peek_24dp)
			.setWhen(System.currentTimeMillis())
			.setContentTitle("GM_test")
			.setContentText("紀錄航跡-TODO");
	// 建立通知物件
	Notification notification = builder.build();
	// 使用BASIC_ID為編號發出通知
	manager.notify((int) id, notification);
}
```


PendingIntent 寫法(不確定):
=====

<http://blog.csdn.net/yuzhiboyi/article/details/8484771>

IntentService
<http://stackoverflow.com/questions/30141631/send-location-updates-to-intentservice>
<http://stackoverflow.com/questions/33735536/fusedlocationapi-with-pendingintent-for-background-location-updates-unable-to-r>

BroadcastReceiver
<http://stackoverflow.com/questions/34252744/fetching-location-using-pendingintent-returns-null-all-the-time>

[Google Doc](https://developers.google.com/android/reference/com/google/android/gms/location/FusedLocationProviderApi#requestLocationUpdates(com.google.android.gms.common.api.GoogleApiClient,%20com.google.android.gms.location.LocationRequest,%20android.app.PendingIntent))

```java
private void sendNotify2(Context context){
	//获取通知管理器
	NotificationManager mNotificationManager = (NotificationManager) getSystemService(Context.NOTIFICATION_SERVICE);
	long when = System.currentTimeMillis() + 15000;//通知发生的时间为系统当前时间 + 5000

	Intent openintent = new Intent(this, MapsActivity.class);
	PendingIntent contentIntent = PendingIntent.getActivity(this, 0, openintent, 0);//当点击消息时就会向系统发送openintent意图

	//新建一个通知，指定其图标和标题
	NotificationCompat.Builder builder =
			new NotificationCompat.Builder(context);
	builder.setSmallIcon(R.drawable.ic_double_peek_24dp)
			.setWhen(when)
			.setDefaults(Notification.DEFAULT_SOUND)
			.setAutoCancel(true)
			.setContentIntent(contentIntent);

	Notification notification = builder.build();
	mNotificationManager.notify(0, notification);//第一个参数为自定义的通知唯一标识
}
```