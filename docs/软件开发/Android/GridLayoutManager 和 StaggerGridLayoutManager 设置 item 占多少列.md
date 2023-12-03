---
title: GridLayoutManager 和 StaggerGridLayoutManager 设置 item 占多少列
date: 2018-2-24 17:04:25
categories: [笔记]
tags: [Android]
---

# GridLayoutManager

重写`onAttachedToRecyclerView`

```java
private GridLayoutManager gridManager;
@Override
public void onAttachedToRecyclerView(RecyclerView recyclerView) {
  super.onAttachedToRecyclerView(recyclerView);
  RecyclerView.LayoutManager manager = recyclerView.getLayoutManager();
  if (manager instanceof GridLayoutManager) {
    gridManager = ((GridLayoutManager) manager);
    if (mGridSpanSizeLookup == null) {
      mGridSpanSizeLookup = new GridSpanSizeLookup();
    }
    gridManager.setSpanSizeLookup(mGridSpanSizeLookup);
  }
}
```

# StaggeredGridLayoutManager

StaggeredGridLayoutManager中没有setSpanSizeLookup方法，但是StaggeredGridLayoutManager.LayoutParams中有setFullSpan方法可以达到同样的效果。

重写onViewAttachedToWindow