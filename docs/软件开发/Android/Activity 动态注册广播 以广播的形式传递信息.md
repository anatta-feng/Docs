---
title: Activity 动态注册广播，以广播的形式传递信息
date: 2017-03-8 12:15:00
categories: [笔记] 
tags: [Android]
---

Activity 动态注册广播，以广播的形式传递信息

## 动态注册

```java
registerReceiver(new BroadcastReceiver() {
			@Override
			public void onReceive(Context context, Intent intent) {
				BaseActivity.this.finish();
			}
		}, new IntentFilter("myaction"));
```

## 调用

```java
Intent intent = new Intent();
intent.setAction("myaction");
sendBroadcast(intent);
```

# 应用场景

批量结束 Activity，却不结束进程的时候

### 还有一种更好的方法

将 Activity 的实例保存在一个 List 里。需要全部退出的时候就遍历，`finish();`

```java
class MyApplication extends Application {
  private List<Activity> list;
  public void addActivity(Activity activity) {
    list.add(activity);
  }
  public void onTerminate() {
    for(Activity activity : list) {
      activity.finish();
    }
  }
}
```

但是不知道会不会引起内存泄漏