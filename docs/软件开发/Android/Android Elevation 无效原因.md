---
title: Android Elevation 无效原因
date: 2018-2-25 17:04:25
categories: [笔记]
tags: [Android]
---
1. 控件必须设置背景色，且不能为透明。
2. 阴影是绘制于父控件上的，所以控件与父控件的边界之间需有足够空间绘制出阴影才行。
3. 有网友提出图片尽量使用. png, 防止图片过大导致 oom 或者 elevation 失效
4. 经过本人测试，除了上述原因外，还有：background 是图片时、background 直接设置具体颜色值时容易无效如：#ffaacc，background 是 shape 时效果最好
5. 设置 elevation 的 View 最好是 ViewGroup 子类