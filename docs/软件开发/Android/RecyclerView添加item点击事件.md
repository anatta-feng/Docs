---
title: RecyclerView 添加 Item 点击事件
date: 2016.11.22 20:55
categories: [笔记]
tags: [Android]
---

# RecyclerView 添加 Item 点击事件

> 这个东西搞了我大半天的时间，网上也查了蛮多资料
>
> 其实原理就是给 RecyclerView 的根 View 设置一个点击事件，然后再获取到 View 里的子控件再进行设置点击事件

目前掌握的还是很浅

<!-- more -->

## 需要自定义接口实现点击回调

传入的两个参数第一个 View 是必传的，用来判断哪个 View 被点击，第二个参数 position 是为了获取到点击了第几个 Item

```java
private interface MyItemOnClickListener {
	// 模仿 ListView 的 onItemClick
	void onItemClick(View v, int position);
}
```

## 在 Adapter 中暴露设置监听接口

因为现在将监听方法在 Adapter 中实现，所以需要在 Adapter 中暴露相关接口，以便后续进一步详细设置

```java
public void setListener(MyItemOnClickListener listener) {
	this.listener = listener;
}
```

## 让 MyViewHolder 实现 OnClickListener 接口

```java
class MyViewHolder extends ViewHolder implements OnClickListener {
	public MyViewHolder (Context context, MyItemOnClickListener listener) {
       	super(context);
	}
  
	@Override
 	public void onClick(View v) 
 	}
}
```

## 设置 View 点击事件

由于 ViewHolder 有 RecyclerView 的根布局，所以先给根布局设置点击事件，然后在点击事件中再详细设置

```java
public void onClick(View v) {
	if(listener != null) {
    	/**
        * 模仿系统点击事件的触发，如果根布局被点击，则调用自定义的点击事件
        * 系统是获取事件后触发回调，消息从底层一直传递给 Activity，然后传
        * 递给 View ，View 调用 performClick() 方法，执行点击事件
        **/
    	listener.onItemClick(v, getPosition())
	}
}
```

### 最后在 Activity 中实现进一步设置

```java
public class MainActivity extends Activity {
  
	...
      
	@Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        
        initData();
		initView();
      	mAdapter = new NewsAdapter(this, mData);
		mAdapter.setOnItemClickListener(new MyItemOnClickListener() {

			@Override
			public void onItemClick(View view, int position) {
				...
			}
		});
    	...
    }
}
```



# 坑

如果在 Adapter 中实现 OnClickListener 接口，则 getPosition() 方法只会获取到最后一个 position，并且不会改变