---
title: Android Studio筆記
comments: false
categories: [Coding, Android]
tags: [Android]
date: 2017-02-13 22:29:25
---

Shortcuts
======

[一些實用快捷鍵](http://greenrobot.me/android-dev-tool/android-studio-dev-tips-1/)

Alt  + insert: list general methods(getter/setter...)
Ctrl + O     : list override methods
Alt  + Enter : Import class / tag public, private...
Shift + F6   : Rename/Refactor
Ctrl + Shift + F7: Show all usage
Ctrl + Y     : Delete whole line
Shift + Alt + ↑ / ↓ : move up/down by one line
Ctrl + P     : 在functoin的括號內時，可顯示輸入型別
Ctrl + Alt + O : 刪除沒用的import
F2           : jump between syntax error
Ctrl + Alt + up/down: jump between compiler error
Ctrl + Q     : See document

<br>
* * *

常用函式
=====
取得時間
```java
new Date(System.currentTimeMillis())
```

<br>
* * *

Namespace
======
```
xmlns:app="schemas.android.com/apk/rest-auto"
```

<br>
* * *

Resource
======
**Monokai theme**
<https://github.com/benmarten/MonokaiAndroidStudio>
Setting->Editor->Font adjust font and size

<br>
* * *

gradle解釋
=====
概覽
<http://www.androidchina.net/4300.html>

查看package
<http://stormzhang.com/devtools/2015/01/05/android-studio-tutorial5/>

自己做Library
<https://github.com/codepath/android_guides/wiki/Building-your-own-Android-library>