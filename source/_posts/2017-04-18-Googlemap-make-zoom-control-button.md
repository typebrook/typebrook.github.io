---
title: 自製Google Maps Api按鈕
comments: true
categories: [Coding, Android]
tags: [Android, Google map]
date: 2017-04-18 19:54:34
---

王天利前輩寫的綠野遊蹤中，除了原本Google Maps Api的縮放按鈕(Zoom Control Button)以外，在上面還多加了一個TextView顯示目前的縮放層級，在放大縮小時可以有所參考，這個細節實在很棒! 於是我決定也來實作一下。

不過，在[Google Maps Api][ApiDoc]的官方文件中並沒有找到相關說明，看來沒這麼容易啊...

![](http://i.imgur.com/illZbyq.jpg)

放大仔細來看，可以發現這個新按鈕和原生按鈕有所差別，邊緣較為模糊，不過半透明的效果和顏色、大小都有到位，猜測應該是前輩自己刻的。(再仔細看，原生按鈕只有`+` `-` 和`定位`，其它按鈕的邊錄放大後都相對較模糊。

![](http://i.imgur.com/2BNU1QI.png)

那麼強迫症如我是否該效法一下，也來自己刻一個呢? 開玩笑，當然是找找StackOverflow有沒有解答啦!

**正解在此:**
<http://stackoverflow.com/questions/22457429/android-google-maps-button-style>

看來是利用apktool來解析檔案，在`res/drawable`中找到相關資源。更棒的是，解答者還直接提供這些資源檔的下載!

* * *

解壓縮後，是各種大小的按鈕圖片以及xml樣式檔，不囉嗦，直接全丟到`res/`裡面，再使用`android:background`引入。

這邊就是自訂的TextView啦，雖然解答者是說寬高為38dp，不過實測是42.5dp才會等同於原生按鈕，接著再調整位置就行! 至於`Shadow padding`就不管了。

```xml
<TextView
	android:id="@+id/zoom"
	android:layout_width="42.5dp"
	android:layout_height="42.5dp"
	android:layout_alignParentRight="true"
	android:layout_alignParentBottom="true"
	android:layout_marginBottom="92dp"
	android:layout_marginRight="9.8dp"
	android:background="@drawable/mapbutton_background"
	android:gravity="center"
	android:textSize="20dp"
	android:text=""/>
```

接著實作`GoogleMap.OnCameraMoveListener`，在畫面變動時更新TextView即可，這邊就不再贅述。

![](http://i.imgur.com/VU8kDtc.jpg "鏘鏘! 做出來了!")

![](http://i.imgur.com/5ubG9w4.png "邊緣和下方的按鈕一樣，Good!")



[ApiDoc]: https://developers.google.com/maps/documentation/android-api/controls?hl=zh-tw
