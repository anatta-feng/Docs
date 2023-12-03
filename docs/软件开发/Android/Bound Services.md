---
title: Bound Services
date: 2017-8-7 17:05:25
categories: [笔记]
tags: [Android, AIDL, Service]
---

# Basic

绑定服务是 Service 类的一个实现，允许其他应用去绑定自身，并且产生交互。

要提供绑定服务，必须实现`onBind()`回调方法，这个方法的返回值`IBinder`对象定义了客户端与服务交互的编程接口。

客户端通过调用`bindService()`方法来绑定服务。当他调用的时候，必须实现`ServiceConnection`，**ServiceConnection** 会监控与 Service 的连接。

> 三种方法，各有适用场景
>
> * 扩展 Binder 类
>   * 如果服用仅供应用本地使用，不需要跨进程工作
> * 使用 Message
>   * 如果需要跨进程，但是不需要并发请求，因为 Messager 会在单一线程中创建包含所有请求的队列。
> * AIDL
>   * 如果应用需要跨进程并且考虑并发请求。



# 创建 Bound Service

在创建一个 Bound Service 的时候必须提供一个用于客户端与服务交互的编程接口， IBinder。有三种方法实现 IBinder。

* 扩展 Binder 类
  * 如果 service 只是用于自己应用内部，那么应该采用扩展 Binder 的形式在`onBind()`方法中返回他的一个实例来创建接口。客户端收到 Binder 后可以利用他直接访问 Binder ，乃至于 Service 中可用的公共方法
* 使用 Messenger
  * 如果 service 需要跨进程工作，那么使用**Messenger**为服务创建接口。这种方式下，Service 定义一个响应不同类型的 Message 对象的 Handler。这个 Handler 是 Messenger 能够与客户端传递 IBinder 的基础，客户端可以通过 Message 对象向 service 发送命令。另外，客户端还可以定义自己的 Messenger，以便 service 回传信息。
  * 这是实现 IPC 通信最简单的方式，因为 Messenger 会在单一线程中创建所有请求队列，所以不需要考虑线程安全
* 使用 AIDL

# 实例 Demo

## 扩展 Binder 类

* 在 Service 里创建一个满足下面任一条件的 Binder
  * 包含客户端可调用的公共方法
  * 返回当前 Service 的实例，其中包含客户端可调用的公共方法
  * 返回由 Service 承载的其他类的实例，其中包含客户端可以调用的公共方法
* 从`onBind()`方法返回 Binder 的实例
* 在客户端中，从`onServiceConnected()`回调方法中接收Binder，并使用提供的方法绑定服务

> Demo

```java
public class LocalService extends Service {
  private final IBinder mBinder = new LocalBinder();
  private final Random mGenerator = new Random();
  
  public class LocalBinder extends Binder {
    LocalService getService() {
      return LocalService.this;
    }
  }
  
  public IBinder onBind(Intent intent) {
    return mBinder;
  }
  
  public int getRandomNumber() {
    return mGenerator.nextInt(100);
  }
}
```

LocalBinder 提供`getService()`方法，可以获取 LocalService 的当前实例，这样客户端就可以调用服务中的公共方法。比如`getRandomNumber()`方法就可以被客户端调用。

> 客户端代码

```java
public class BindingActivity extends Activity {
  LocalService mService;
  boolean mBound = false;
  protected void onCreate(Bundle saveInstanceState) {
    super.onCreate(saveInstanceState);
    setContentView(R.layout.main);
  }
  protected void onStart() {
    super.onStart();
    Intent intent = new Intent(this, LocalService.class);
    bindService(intent, mConnection, Context.BIND_AUTO_CREATE);
  }
  protected void onStop() {
    super.onStop();
    if(mBound) {
      unbindService(mCOnnection);
      mBound = false;
    }
  }
  
  public void onButtonClick(View v) {
    if(mBound) {
      int num = mService.getRandomNumber();
      Toast.makeText(this, "number: " + num, Toast.LENGTH_SHORT).show();
    }
  }
  
  private ServiceConnection mConnection = new ServiceConnection {
    public void onServiceConnected(ComponentName className, IBinder service) {
      LocalBinder binder = (LocalBinder) service;
      mService = binder.getService();
      mBound = true;
    }
    
    public void onServiceDisconnected(ComponentName arg0) {
      mBound = false;
    }
  }
}
```

## Messenger

略过，只记录最重要的：

```java
// 客户端绑定 service，跨进程绑定
Intent intent = new Intent();
intent.setComponent(new ComponentName(packageName, className));
```

步骤

* Service 实现一个`Handler`，用来接收客户端的回调调用。
* `Handler`用于创建 `Messenger` 对象（对 `Handler` 的引用）
* `Messager` 创建一个 `IBinder`，`Service` 通过 `onBind()` 使其返回客户端
* 客户端使用 `IBinder` 将 `Messager`（引用 `Service` 的 `Handler`）实例化，然后使用后者将 `Message` 对象发送给 `Service`

## Service 代码

```java
public class MessengerService extends Service {
    private Messenger messenger;
    static final int MSG_SAY_HELLO = 1;
    class IncomingHandler extends Handler {
        public void handleMessage {
            ...
        }
    }
    public IBinder onBind(Intent intent) {
        return messenger.getBinder();
    }
}
```

## 客户端代码

```java
private ServiceConnection mConnection = new ServiceConnection() {
    public void onServiceConnected(ComponentName className, IBinder service) {
        //这个对象可以向目标 Service 发送消息
        mService = new Messenger(service);
        mBound = true;
    }

    public void onServiceDisconnected(ComponentName className) {
        mService = null;
        mBound = false;
    }
};
```

如果需要 Service 回复消息，就需要在客户端发送消息的时候携带上客户端本地 `Messenger`

```java
Messenger localMessenger = ...;
Messenger remoteMessenger = ...;
Message msg = Message.obtain();
msg.replyTo = localMessenger;
remoteMessenger.send(msg);
```

# 绑定到服务

客户端可通过调用`bindService()`绑定服务。Android 系统随后调用服务的`onBind()`方法，该方法返回用于与服务交互的 IBinder，绑定是一步的。

`bindService()`会立即返回，"不会"使 IBinder 返回客户端。要接收 IBinder，客户端必须创建一个 ServiceConnection 实例，并将其传递给`binService()`，ServiceConnection 包含一个回调方法，系统通过这个方法来传递 IBinder。

> 只有 Activity，Service 和内容提供器可以绑定到服务，无法用广播接收器绑定服务

想要从客户端绑定到服务，必须满足下面几点：

1. 实现 ServiceConnection
2. 调用 bindService()，传递 ServiceConnection 实例
3. 当系统调用 `onServiceConnected()` 方法时，可以使用接口定义的方法开始调用服务
4. 与服务断开连接，调用`unbindService()`

# AIDL

