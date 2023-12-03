---
title:RecyclerView.LayoutManager 源码学习
date:2017-2-21 17:05:25
categories:[笔记]
tags:[Android, LayoutManager]
---

# 准备

## 方法概览

### OnLayoutChildren()

LayoutManager 的主入口，在初始化布局的时候调用，当适配器的数据改变的时候（或者适配器被更换），会再次调用。

作用就是在初始化的时候放置 item，直到填满布局为止

### canScrollHorizontally() && canScrollVvertically()

 在想滚动的方向对应的方法的返回值返回`true`，反之返回`false`

### scrollHorizontallyBy() && scrollVerticallyBy()

实现滚动的逻辑。RecyclerView 已经处理了触摸事件，当上下左右滑动的时候`scrollHorizontallyBy() & scrollVerticallyBy()`会传入此时位移的偏移量，根据这个偏移量需要完成：

1. 将所有的子视图移动到适当的位置
2. 决定移动视图后 添加/移除 视图
3. 返回滚动的实际距离。RecyclerView 会根据它判断是否触碰到边界