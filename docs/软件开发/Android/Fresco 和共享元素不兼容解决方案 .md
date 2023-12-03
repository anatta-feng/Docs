---
title: Fresco 和共享元素不兼容解决方案 
date: 2018-3-1 17:04:25
categories: [笔记]
tags: [Android, Fresco]
---

#不兼容默认的 Transition

需要指定 Transition 为 ChangeBounds

在 style 中设置

```xml
<item name="android:windowSharedElementEnterTransition">@transition/share_element_transition</item>
<!--<item name="android:windowSharedElementExitTransition">@transition/share_element_transition</item>-->
<item name="android:windowSharedElementReturnTransition">@transition/share_element_transition</item>
<item name="android:windowSharedElementReenterTransition">@transition/share_element_transition</item>
```

share_element_transition为

```xml
<?xml version="1.0" encoding="utf-8"?>
<transitionSet xmlns:android="http://schemas.android.com/apk/res/android">
    <changeBounds />
</transitionSet>
```

# 在 Android N 平台会出现返回上一级后 View 消失的问题

在 startActivity 调用前，设置

```kotlin
setExitSharedElementCallback(object : SharedElementCallback () {
    override fun onSharedElementEnd(sharedElementNames: MutableList<String>, sharedElements: MutableList<View>, sharedElementSnapshots: MutableList<View>) {
        super.onSharedElementEnd(sharedElementNames, sharedElements, sharedElementSnapshots)
        sharedElements
        .filterIsInstance<SimpleDraweeView>()
        .forEach { it.visibility = View.VISIBLE }
    }
})
```

