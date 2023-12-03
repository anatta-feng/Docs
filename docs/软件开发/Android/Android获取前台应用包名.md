---
title: Android获取前台应用包名
date: 2017-1-22 17:05:25
categories: [笔记]
tags: [Android]
---

> Android L 对这些权限做了回收，所以在 Android L 前后所使用的方法是不一样的。

## Android L 之前

### 方法一：getRunningTasks()

这种方法不仅可以获取到前台进程包名，还可以获取到前台 activity 名。

```java
public String getForgroundActivity() {
  ActivityManager manager = (ActivityManager)context.getSystemService(Context.ACTIVITY_SERVICE);
  if(manager.getRunningTasks(1) == null) {
    return null;
  }
  ActivityManager.RunningTaskInfo task = manager.getRunningTask(1).get(0);
  if(task == null) {
    return null;
  }
  String pkgName = task.topActivity.getPackageName();
  return pkgName;
}
```

### 方法二：getRunningAppProcesses()

这种方法只能获取前台包名

```java
public String getForground() {
  ActivityManger manager = (ActivityManager)context.getSystemService(Context.ACTIVITY_SERVICE);
  List<RunningAppProcessInfo> lr = manager.getRunningAppProcesses();
  if(lr == null) {
    return null;
  }
  for(RunningAppProcessInfo ra : lr) {
    if(ra.importance == RunningAppProgressInfo.IMPORTANCE_VISIBLE || ra.importance == RunningAppProcessInfo.IMPORTANCE_FOREGROUND) {
      return ra.processName;
    }
  }
  return null;
}
```

---

## Android L 之后

> 在 Android L 上，Google 回收了这两个权限，只能获取到自身的包名

Android L 上 Google 添加了一个权限检查。

想要通过这个权限检查只有两种情况：

* 拥有 REAL_GET_TASKS 权限授权
* 拥有 GET_TASKS 权限授权，同时自己是 privileged app，就是说安卓在 `/system/priv-app/` 目录下的 app

REAL_GET_TASKS 只有系统签名的 app 可以申请。

```xml
<permission android:name="android.permission.GET_TASKS"  
    android:permissionGroup="android.permission-group.APP_INFO"  
    android:protectionLevel="normal"  
    android:label="@string/permlab_getTasks"  
    android:description="@string/permdesc_getTasks" />  

<permission android:name="android.permission.REAL_GET_TASKS"  
    android:permissionGroup="android.permission-group.APP_INFO"  
    android:protectionLevel="signature|system"  
    android:label="@string/permlab_getTasks"  
    android:description="@string/permdesc_getTasks" />  
```

所以要获取前台应用包名只能曲线救国了。

用`usage statistics API`这个 API 是系统用来统计 app 使用情况的，包含了每个 app 最近一次被使用的时间。我们只需要找出最近的，时间最短的那个，就是当前应用了。

```java
private String getForegroundApp() {
  UsageStatsManager stats = (UsageStatsManager)context.getSystemService(Context.USAGE_STATS_SERVICE);
  long time = System.currentTimeMills();
  List<UsageStats list = stats.queryUsageStats(UsageStatsManager.INTERVAL_BEST, 0, time);
  UsageEvents usageEvent = stats.queryEvent(isInit ? 0; time - 5000, time);
  if(usageEvent == null) {
    return null;
  }
  
  UsageEvents.Event event = new UsageEvents.Event();
  UsageEvents.Event lastEvent = null;
  while(usageEvent.getNextEvent(event)) {
    if(event.getPackageName() == null || event.getClassName() == null) {
      continue;
    }
    if(lastEvent == null || lastEvent.getTimeStamp() < event.getTimeStamp()) {
      lastEvent = event;
    }
  }
  if(lastEvent == null) {
    return null;
  }
  return lastEvent.getPackageName();
}
```

> 参考：http://blog.csdn.net/turkeycock/article/details/50765576

---

## Android 彻底关闭自身 app

`Process.killProcess(int pid)`

实例可以这么用：

`Process.killProcess(android.os.Process.myPid());`

### 扩展

其实可以获取到所有的正在运行的进程，获取到他的 pid，然后就这个方法去 kill 掉他，简单的内存清理器。这里需要结合上面的代码

```java
 public int[] getPids(Context ctx) {
   ActivityManager am = (ActivityManager)ctx.getSystemService(Context.ACTIVITY_SERVICE);
   List<RunningAppProcessInfo> list = am.getRunningAppProcesses();
   // list 里面现在是一个正在运行的集合，同样只适用4.4以下，5.0以上需要特殊处理
   int[] pids = new int[list.size()];
   for(int i=0; i<list.size(), i++) {
     pids[i] = list.get(i).pid;
   }
 }
```

