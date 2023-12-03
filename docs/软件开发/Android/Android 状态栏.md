# Android 状态栏

* NotificationManager 的使用
* Notification 的使用
* Notification.Bulider 的使用

想要在状态栏展示消息，一般是在广播接收器或者服务这些后台里进行的，当然，活动里也可以，不过这样的应用场景比较少

## 获取 NotificationManager

Android 中管理类很重要，比如 ActivityManager、PackageManager 等，而获取这些类的实例几乎都是一个套路

```java
NotificationManager = conext.getSystemService(Context.NOTIFICATION_SERVICE);
```

## 获取 Notification

我在一些书或者博客中看到他们说实例化 Notification 类应该这样

```java
Notification n = new Notification(int icon, String text, long time);
n.setLatestEventInfo(Context, String text, String text, null);
```

但是我翻看了 API 文档，却并没有发现 Notification 中有这个方法。而且上述实例化用的构造器在 API 11 的时候已经废弃，我在 API 文档中发现了官方推荐的方法

```java
Notification n = new Notification.Builder(context)
				.setContentTitle(text)
				.setContextText(text)
				.setSmallIcon(int icon)
				.setLargeIcon(Bitmap bitmap)
				.build();
```

## 最后

我们实例化出来的 NotificationManager 还没用呢，现在该用上了

```java
manager.notify(int id, Notificaton n);
```

---

## 补充

### addAction()

```Notification.Builder().addAction();```

这个方法是给通知加入一个子项

![](https://ww3.sinaimg.cn/large/006tNc79jw1fbd7i6gwimj30u01hcju2.jpg)

这个方法有三个参数

* icon
* title
* intent

而这个方法在 API 23 被废弃了，换成了另一个方法，只有一个参数

```addAction(Notification.Action action);```

这个其实是对 action 做了一个封装，其构造器与之前 addAction 是一样的

---

然而又发现 Notification.Action 的构造器也被废弃了，谷歌推荐

Notification.Action.Builder 的构造器，而且直接传入 资源 id 的构造器也被废弃（API 文档中没有注明废弃，但是 AS 中标识废弃），需要封装为 Icon 对象

* icon
* title
* intent

---

### 对状态栏消息添加点击事件

```setContentIntent```

注：一般通知栏消息点击后会消失，当然如果你不做逻辑处理的话，他是不会自动消失的我们需要做的是，在响应逻辑中一开始就调用 NotificationManager 的 cancel 方法

```java
NotificationManager manager;
manager.cancel(int id);
```

这个 id 就是我们在 notify 的时候传入的 id。

---

### 对状态栏消息设置布局

Notification.Builder.setContent(View view);