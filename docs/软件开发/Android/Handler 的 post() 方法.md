---
title: Handler 的 post() 方法
date: 2017-5-2 10:05:25
categories: [笔记]
tags: [Android]
---

# Handler 的 post() 方法

> 1. post() 方法和 sendMessage() 方法有什么区别？
> 2. 为什么要用 post() 方法？

## post() 方法的调用

```java
new Handler().post(new Runnable() {
  public void run() {
    Toast("post");
  }
});
```

仅看代码很容易让人以为 post() 方法是启动了一个子线程，因为他的参数是一个 Runnable，其实不然，因为只调用了`run()`方法，没有调用`start()`，他其实是在主线程调用的。

## post() 和 sendMessage() 的区别

其实两个本质上没有区别，如果阅读了 Handler 的源码，就会发现其实两个都是调用`sendMessageDelayed()`方法来实现的。不同只是调用上的不同。

## 为什么要用 post()

我个人目前的理解，`sendMessage()`是用来处理多个线程消息的，比如：

```java
Handler mHandler = new Handler() {
  public void handlerMessage(Message msg) {
    switch(msg.what) {
      case 1:
        Toast("thread 1");
        break;
      case 2:
        Toast("thread 2");
        break
    }
  }
}
```

相信这种用法大家都烂熟于心了吧，这就是用`sendMessage()`方法来实现的

`post()`是用来处理只有一个单线程的

```java
new Thread(new Runnable() {
  public void runn() {
    String text = "thread";
    new Handler.post(new Runnable() {
      public void run() {
        textView.setText(text);
      }
    });
  }
}).start();
```

这样虽然看上去是在子线程操作 UI，但是其实在 post 方法里将 Runnable 包装为一个 Message，插入到消息队列，然后在主线程中处理，所以这样的调用是没错的。

但是并不推荐这样用，因为迷之缩进毁一生，我们要严格要求自己，保持良好的代码风格。

自己对消息机制的原理还不熟，多线程调度更是菜鸟，加油！