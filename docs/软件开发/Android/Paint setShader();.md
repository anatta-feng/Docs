---
title: Paint setShader();
date: 2017-2-23 14:30
categories:
  - 工作
tags:
  - View
---
# Paint setShader();

如果 BitmapShader 的尺寸小于 Paint 画出的图像的大小，那么 Shader 就会导致右边缘和下边缘出现类似拉丝的效果，只需要微调下 Bitmap 的缩放尺寸，让 Bitmap 的尺寸大于等于 View 的大小就不会出现这种情况。

工作中出现的问题是因为传入的 Rect 尺寸有问题导致计算的缩放系数有问题，但是我没有细看这部分代码，只是自己手动调节了一个合适的值。回头需要再认真ReView一下