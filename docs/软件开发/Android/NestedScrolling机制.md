---
title: NestedScrolling机制
date: 2018-2-22 17:04:25
categories: [笔记]
tags: [Android]
---
> 最近在写自己 App 的时候，遇到界面上的问题，然后找到了 NestedScrolling 机制，特此记录

# 简介

NestedScrolling 其实是一套 View 事件传递机制，他和传统的 View 事件传递的区别就在于：子 View  在处理事件的前后，可以让父布局插入。

# 接口

## NestedScrollingChild

```java
/**
 * 向父布局分发一个惯性滑动事件
 **/
boolean dispatchNestedFling(float velocityX, float velocityY, boolean consumed)
```

```java
/**
 * 在子View处理事件前，分发给父布局
 **/
boolean dispatchNestedPreFling(float velocityX, float velocityY)
```

```java
/**
 * 在消费任何事件之前分发滚动事件
 **/
boolean dispatchNestedPreScroll(int dx, int dy, int[] consumed, int[] offsetInWindow)
```

```java
/**
 * 分发滚动事件
 **/
boolean dispatchNestedScroll(int dxConsumed, int dyConsumed, int dxUnconsumed, int dyUnconsumed, int[] offsetInWindow)
```

```java
/**
 * 如果有一个 Nested Parent 则返回 true
 **/
boolean hasNestedScrollingParent()
```

```java
/**
 * 获得是否开启了嵌套滚动
 **/
boolean isNestedScrollingEnabled()
```

```java
/**
 * 设置是否开启嵌套滚动
 **/
void setNestedScrollingEnabled(boolean enabled)
```

```java
/**
 * 沿给定的轴线开始嵌套滚动
 **/
boolean startNestedScroll(int axes)
```

```java
/**
 * 停止当前的滚动
 **/
void stopNestedScroll()
```

### startNestedScroll

子 View 开启整个流程。

### dispatchNestedProScroll

```java
boolean dispatchNestedPreScroll(int dx, int dy, int[] consumed, int[] offsetInWindow)
```

子View接收到`MontionEvent.ACTION_MOVE`时，通知父布局滑动的距离。`consumed`和`offsetInWindow`记录父布局消费掉的scroll长度和子View的窗体偏移量。如果这个事件没有被消费完，子View处理剩下的值。

由于窗体进行了移动，如果记录了手指最后的位置，需要根据`offsetInWindow`计算偏移量，才能保证下一次的touch事件的计算是正确的。

### dispatchNestedScroll

想父View汇报滚动情况，包括子View消费的部分和子View没有消费的部分。

如果父View接收了此View的滚动事件，进行了部分消费，啧返回True，否则返回False

此函数一般在子View处理Scroll后调用

### stopNestedScroll

结束整个流程

## NestedScrollingParent

```java
/**
 * 返回当前布局嵌套滚动的坐标轴
 **/
int getNestedScrollAxes()
```
```java
/**
 * 惯性滑动
 **/
boolean onNestedFling(View target, float velocityX, float velocityY, boolean consumed)
```
```java
/**
 * 惯性滑动前的回调
 **/
boolean onNestedPreFling(View target, float velocityX, float velocityY)
```
```java
/**
 * 子View滚动前的回调
 **/
boolean onNestedPreScroll(View target, int dx, int dy, int[] consumed)
```
```java
/**
 * 滚动
 **/
void onNestedScroll(View target, int dxConsumed, int dyConsumed, int dxUnconsumed, int dyUnconsumed)
```
```java
/**
 * 响应子View的滚动
 **/
void onNestedScrollAccepted(View child, View target, int axes)
```
```java
/**
 * 决定是否接收子View的滚动
 **/
boolean onStartNestedScroll(View child, View target, int axes)
```
```java
/**
 * 滚动结束的回调
 **/
void onStopNestedScroll(View target)
```

# 流程

子View和父View的流程是相对应的

| 子View                  | 父View                                      |
| ----------------------- | ------------------------------------------- |
| startNestedScroll       | onStartNestedScroll、onNestedScrollAccepted |
| dispatchNestedPreScroll | onNestedPreScroll                           |
| dispatchNestedScroll    | onNestedScroll                              |
| stopNestedScroll        | onStopNestedScroll                          |

子View发起调用，父View响应回调

# 备注

在Child中最需要关注的是`dispatchNestedPreScroll`中的`consumed`参数

```java
boolean dispatchNestedPreScroll(int dx, int dy, int[] consumed, int[] offsetInWindow)
```

它是一个`int`型的数组，长度为 2，第一个元素是父 view 消费的`x`方向的滚动距离；第二个元素是父 view 消费的`y`方向的滚动距离，如果这两个值不为 0，则子 view 需要对滚动的量进行一些修正。



# 参考
[Android 嵌套滑动机制（NestedScrolling）](https://segmentfault.com/a/1190000002873657)