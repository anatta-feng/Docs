---
title: Bitmap Drawable 互转
date: 2017-9-11 17:05:25
categories: [笔记]
tags: [Android]
---

> 做个记录

# Bitmap -> Drawable

```java
Drawable drawable = new BitmapDrawable(bitmap);
```



# Drawable -> Bitmap

```java
Resources res = getResources();
Bitmap bitmap = BitmapFactory.decodeResource(res, R.drawable.test);
```

