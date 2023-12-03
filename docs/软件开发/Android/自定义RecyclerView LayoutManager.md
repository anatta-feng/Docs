---
title: 自定义 RecyclerView LayoutManager
date: 2017-11-15 23:05:25
categories: [笔记]
tags: [Android, RecyclerView, LayoutManager]
---

# RecyclerView 简介

RecyclerView 很万能，它主要由**LayoutManager**，**ItemDecoration**， **ItemAnimator**， **Adapter**， **ViewHolder**组成

* RecyclerView.LayoutManager
  * 负责 Item 视图的布局的显示和管理
* RecyclerView.ItemDecoration
  * 给每一项 Item 视图添加子View，可以画分割线之类的
* RecyclerView.ItemAnimator
  * 负责 处理数据添加或者删除的时候的动画
* RecyclerView.Adapter
  * 为每一项Item 创建视图
* RecyclerView.ViewHolder
  * 承载 Item 视图的子布局

# LayoutManager 接口介绍

LayoutManager 只有一个抽象方法：`generateDefaultLayoutParams()`

## 其他重要方法

* `onLayoutChildren(RecyclerView.Recycler recycler, RecyclerView.State state)`
  * 这个方法是进行 item 布局时执行的，他决定了 item 放在什么位置，`recycler` 是 RecyclerView 的回收池，`state` 是RecyclerView 的状态信息。当 LayoutManager 初始化的时候就会执行这个方法进行子对象的布局工作，当界面刷新的时候也会调用这个方法。**该方法在初始化的时候会调用两遍**
* `canScrollVertically() || canScrollHorizontally()`
  * 是否可以竖直||水平滑动，返回值为 bool
* `scrollVerticallyBy(int dy, RecyclerView.Recycler recycler, RecyclerView.State state)`
  * 手指竖直滑动的时候，会调用这个方法来获取 RecyclerView item 的滚动距离，返回值是 int， 同样也有 `scrollHorizontallBy(...)`

## 其他常用 API

* `recycler.getViewForPosition(position)`
  * 获取指定 position 的 View
* `getPosition(View v)`
  * 获取 View 的 position
* `measureChildWithMargins(View child, int widthUsed, int heightUsed)`
  * 测量 View 的宽高，包括 margin
* `layoutDecoratedWithMargins(View child, int left, int top, int right, int bottom)`
  * 将 child 显示在 RecyclerView 上面，`left, top, right, bottom`规定了显示区域
* `detachView(View child)`
  * 临时回收 View
* `attachView(View child)`
  * 将`detachView()`回收的 View 拿出来
* `detachAndScrapAttachedViews(RecyclerView.Recycler recycler)`
  * 用指定的 recycler 临时移除所有添加到 Views
* `detachAndScrapView(View child, RecyclerView.Recycler recycler)`
  * 用指定的 recycler 临时回收 view
* `removeAndRecyclerAllViews(RecyclerView.Recycler recycler)`
  * 移除所有的 View 并且用给的 recycler 回收
* `removeAndRecyclerView(View child, RecyclerView.Recycler recycler)`
  * 移除指定的 view 并且用给的 recycler 回收
* `offsetChildrenHorizontal(int dx)`
  * 水平移动所有的 view，同样也有`offsetChildrenVertical(int dy)`

# LayoutManager 按功能分类接口

## 布局 API

```java
//找 recycler 要一个 child，不关心从哪里来
recycler.getViewForPosition(int)
```

```java
addView(view);//将View添加至RecyclerView中，
addView(child, 0);//将View添加至RecyclerView中，childIndex为0，但是View的位置还是由layout的位置决定，该方法在逆序layout子View时有大用
```

```java
measureChildWithMargins(scrap, 0, 0);//测量View,这个方法会考虑到View的ItemDecoration以及Margin
```

```java
//将ViewLayout出来，显示在屏幕上，内部会自动追加上该View的ItemDecoration和Margin。此时我们的View已经可见了
layoutDecoratedWithMargins(view, leftOffset, topOffset,
                        leftOffset + getDecoratedMeasuredWidth(view),
                        topOffset + getDecoratedMeasuredHeight(view));
```

## 回收 API

```java
detachAndScrapAttachedViews(recycler);//detach轻量回收所有View
detachAndScrapView(view, recycler);//detach轻量回收指定View

// recycle真的回收一个View ，该View再次回来需要执行onBindViewHolder方法
removeAndRecycleView(View child, Recycler recycler)
removeAndRecycleAllViews(Recycler recycler);
```

```java
detachView(view);//超级轻量回收一个View,马上就要添加回来
attachView(view);//将上个方法detach的View attach回来
recycler.recycleView(viewCache.valueAt(i));//detachView 后 没有attachView的话 就要真的回收掉他们
```

## 移动子 View API

```java
offsetChildrenVertical(-dy); // 竖直平移容器内的item 
offsetChildrenHorizontal(-dx);//水平平移容器内的item
```

## 工具 API

```java
public int getPosition(View view)//获取某个view 的 layoutPosition
```

