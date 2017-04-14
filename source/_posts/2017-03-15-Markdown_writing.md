---
title: Markdown寫法
date: 2017-03-15 00:31:03
categories: [Coding, blog]
tags: [架站筆記, hexo]
comments: false
---

外部連結
======
<br>
[標籤外掛(Hexo doc)](https://hexo.io/zh-tw/docs/tag-plugins.html)

<br>[md語法(md doc)](http://markdown.tw/)

<br>[Markdown的正确使用方式](https://unnamed42.github.io/2015-12-02-Markdown%E7%9A%84%E6%AD%A3%E7%A1%AE%E4%BD%BF%E7%94%A8%E6%96%B9%E5%BC%8F.html)

<br>[md語法(gitbooks)](https://wastemobile.gitbooks.io/gitbook-chinese/content/format/markdown.html)

<br>
* * *

項目
======
<br>
1. 項目一(會自動縮排)
2. 項目二
>3. 引言中的項目

{% codeblock %}
1. 項目一(會自動縮排)
2. 項目二
>3. 引言中的項目
{% endcodeblock %}

* * *

引言
======
<br>
>引言
>>引言2

```md
>引言
>>引言2
```

<br>

<blockquote class="blockquote-center">
引言三
</blockquote>

```md
<blockquote class="blockquote-center">
引言三
</blockquote>
```


<br>
* * *

引言(有來源連結)
======
<br>
{% blockquote 蔡日興  https://jtsai.gitbooks.io/forest_deer_hunter/content/section11.html 地圖上不存在的林道 %}
在台灣，爬山的人所說的中級山，指的不是風景優美使人嚮往的高山百岳，也不是到了周末熱鬧滾滾的都市近郊山徑，乃是在二者之間，罕無人煙的廣大蠻荒山野。
{% endblockquote %}

```md
{% blockquote 蔡日興 https://jtsai.gitbooks.io/forest_deer_hunter/content/section11.html 地圖上不存在的林道 %}
在台灣，爬山的人所說的中級山...
{% endblockquote %}
```

<br>
* * *

Link
======
行內連結
This is [link_name](https://www.facebook.com/ "Title") inline link.

```md
This is [link_name](https://www.facebook.com/ "Title") inline link.
```

自動連結
<https://google.com>
<typebrook@gmail.com> (手機可直接送信)

```md
<https://google.com>
<typebrook@gmail.com> (手機可直接送信)
```

<br>
* * *

圖片
======
<br>

![Alt text](http://i.imgur.com/CzWevLy.jpg)


```md
![Alt text](http://i.imgur.com/CzWevLy.jpg)
```

<br>
* * *

程式碼
======
This is syntax of tag
```md
 # Place your favicon.ico to /source directory.
 favicon: [your_new_icon.png]
```

````
```
 # Place your favicon.ico to /source directory.
 favicon: [your_new_icon.png]
```
````
    
This is `code` in line

```md
2. This is `code` in line
```

<br>
* * *

清單
======
*   Lorem ipsum dolor sit amet, consectetuer adipiscing elit.
    Aliquam hendrerit mi posuere lectus. Vestibulum enim wisi,
    viverra nec, fringilla in, laoreet vitae, risus.
*   Donec sit amet nisl. Aliquam semper ipsum sit amet velit.
    Suspendisse id sem consectetuer libero luctus adipiscing
    
```
*   Lorem ipsum dolor sit amet, consectetuer adipiscing elit.
    Aliquam hendrerit mi posuere lectus. Vestibulum enim wisi,
    viverra nec, fringilla in, laoreet vitae, risus.
*   Donec sit amet nisl. Aliquam semper ipsum sit amet velit.
    Suspendisse id sem consectetuer libero luctus adipiscing
```
    
<br>
* * *

強調
======
使用`*`  : *single asterisks*
```md
*single asterisks*
```

使用`_`  : _single underscores_
```md
_single underscores_
```

使用`**` : **double asterisks**
```md
**double asterisks**
```

使用`__` : __double underscores__
```md
__double underscores__
```