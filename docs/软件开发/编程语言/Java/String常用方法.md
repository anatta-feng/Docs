---
title: String常用方法
date: 2016-8-29 12:15:00
categories: [读书笔记] 
tags: [Java]
---
# valueOf 转换为字符串 #
	public static String valueOf(boolean b)

本方法是将其他类型的数据转换为字符串类型。方法形参可以传入各种类型的值：char, int, 数组, 对象的引用 等。

---

# substring #

	str = str.substring(int beginIndex);

截取从首字母长度起 beginIndex 长度的字符串，将剩余字符串赋值给 str。

	str = str.substring(int beginIndex, int endIndex);
截取从 beginIndex 开始至 endIndex 结束的字符串并赋值给 str。

<!-- more -->

---

# indexOf #

	int i = str.indexOf(s);

返回 str 字符串中第一次出现 字符串 s 的位置。