---
title: 自定义 View 中 onMeasure 方法的使用
date: 2016-12-14 20:55
categories:
  - 笔记
tags:
  - Android
---

## 自定义 View 中 onMeasure 方法的使用

> 菜鸟一枚，如果有错误或者不妥的地方希望指正，谢谢

在自定义 View 中有许多方法需要涉及，比如 

* onLayout()
* onMeasuer()
* onDraw()
* ...

这里主要说一下 onMeasure 方法。

<!-- more -->

---

onMeasure 方法是用来测量 View 大小的，他有两个参数

```java
@Override
protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
	super.onMeasure(widthMeasureSpec, heightMeasureSpec);
}

```

这两个参数是可以取到宽高参数的，我们可以从中取出宽高数值，以及宽高的测量模式

```java
MeasureSpec.getSize(int w);
MeasureSpec.getMode(int w);
```

Android 中的测量模式又分三种

```java
/**
 * Measure specification mode: The parent has not imposed any
 * constraint on the child. It can be whatever size it wants.
 */
public static final int UNSPECIFIED = 0 << MODE_SHIFT;

/**
 * Measure specification mode: The parent has determined an exact 
 * size for the child. The child is going to be given those 
 * bounds regardless of how big it wants to be.
 */
public static final int EXACTLY = 1 << MODE_SHIFT;

/**
 * Measure specification mode: The child can be as large as it 
 * wants up to the specified size.
 */
public static final int AT_MOST = 2 << MODE_SHIFT;
```

这三种模式分别是 UNSPECIFIED（可以任意指定大小）、EXACTLY（父控件已经指定子 View 的大小，比如配置文件中明确指定大小，或者 match_parent）、AT_MOST（最大值模式，不规定大小，但是有上限，即父容器的大小，即 wrap_content）

---

在自定义 View 时需要测量 View 大小，所以在重写 onMeasure 方法的时候我们需要对各种模式加以判断，以便在布局文件中使用时有良好的体验

附上我用的方案

```Java
if (widthMode == MeasureSpec.EXACTLY) {
	// 如果测量方式为 EXACTLY 的话就直接使用传进来的参数即可
	width = widthMeasureSpec；
} else {
	// 如果是其他模式则指定自己的大小
	width = (int) (centerRadius * 2 + mWaveStartWidth * 2);
}
```

---

如有谬误，请指正。