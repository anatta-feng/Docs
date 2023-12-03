---
title: Layout Changes Animation
date: 2017-12-6 14:05:25
categories: [笔记]
tags: [Android, 动画]
---
![2017-12-06 14_02_50](https://ws3.sinaimg.cn/large/006tKfTcly1fm70i3coiwg308m0fc76o.gif)

像这样的效果，只需要在xml 中设置`animateLayoutChanges="true"`或者在代码中添加一个`LayoutTransition`对象即可实现在任何 ViewGroup 布局改变时的动画。

目前支持五种状态变化，可以为下面任意一种状态自定义动画：

* APPEARING：容器中出现一个视图
* DISAPPEARING：容器中消失一个视图
* CHANGING：布局改变导致某个视图随之改变，例如调整大小，但不包括添加或者移除视图。
* CHANGE_APPEARING：其他视图的出现导致某个视图改变
* CHANGE_DISAPPEARING：其他视图的消失导致某个视图改变

#Eg.

```xml
<?xml version="1.0" encoding="utf-8"?>  
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    android:layout_width="match_parent"  
    android:layout_height="match_parent"  
    android:orientation="vertical" >  
    <Button   
        android:id="@+id/main_btn"  
        android:layout_width="match_parent"  
        android:layout_height="wrap_content"  
        android:text="添加控件"/>  
    <LinearLayout   
        android:layout_width="match_parent"  
        android:layout_height="match_parent"  
        android:animateLayoutChanges="true"  
        android:id="@+id/main_container"  
        android:orientation="vertical"/>  
</LinearLayout> 
```

此时这个容器改变就已经有动画了，只不过是系统默认的。如果需要自定义动画，就需要在代码中进行设置。

```java
public class MainActivity extends Activity {  
  
    private LinearLayout mContainer;  
  
    @Override  
    protected void onCreate(Bundle savedInstanceState) {
   		super.onCreate(savedInstanceState);  
        setContentView(R.layout.main);  
        mContainer = (LinearLayout) findViewById(R.id.main_container); 
      LayoutTransition transition = new LayoutTransition();
      mContainer.setLayoutTransition(transition);
      //进入动画
      Anitator appearAnim = ObjectAnimator.ofFloat(null, "rotationY", 90f, 0).setDuration(transition.getDuration(LayoutTransition.APPEARING));
      transition.setAnimator(LayoutTransition.APPEARING, appearAnim);
      
      //其他动画类似
      
    }
```

