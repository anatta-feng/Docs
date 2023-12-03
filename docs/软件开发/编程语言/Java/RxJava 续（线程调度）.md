---
title: RxJava 续（线程调度）
date: 2017.4.28 15:20
categories: [笔记]
tags: [代码]
---

# RxJava 续（线程调度）

已经掌握了RxJava的基本使用方法：

> 创建一个 Observable(被观察者)、Observer(观察者)，然后 `subscribe` 一下，订阅关联起来两个就可以执行了。

但是这只是最基础的用法，仅仅能跑起来而已。RxJava的灵魂是异步，看看异步怎么实现。

## Scheduler

线程调度

在不指定现成的情况下，RxJava遵循的是线程不变原则，Observable 在哪个线程产生事件，Observer 就在哪个线程消费事件。如果需要切换线程，就需要 RxJava 的 Scheduler 概念：线程调度管理。

可以使用这两个方法来分别指定事件的产生和消费所在的线程：

* `subscribeOn()`
  * 指定事件产生的线程
* `observerOn()`
  * 指定事件消费所在的线程

### RxJava内置的 Scheduler 

* `Scheudlers.immediate()`
  * 默认的Scheduler，当前线程
* `Scheudlers.newThread()`
  * 启用一个新的线程，并且在这个新的线程产生或者消费事件
* `Scheudlers.io()`
  * 将操作放在一个io线程中
    * 与`newThread()`的区别是`io()`内部实现是用一个无数量上限的线程池，可以重用空闲的线程，效率比`newThread()`高。
    * 但是计算工作放在`io()`里的话会创建不必要的线程，应避免
* `Scheudlers.computation()`
  * 计算时候使用的`Scheduler`。
    * 此处的计算指的是CPU密集型计算（不会被I/O等操作限制性能的计算，如图形计算）
  * 内部实现使用固定的线程池，大小为 CPU 核心数。
  * 不要将 I/O 等操作放入计算线程，会浪费CPU
* Android 专用 `AndroidScheduers.mainThread()`
  * 在 Android 的主线程运行

上代码

```java
Observable.just("just1", "just2")
  //事件在新线程产生
  .subscribeOn(Schedulers.newThread())
  //事件在Android主线程消费
  .observeOn(AndroidScheduers.mainThread())
  .subscribe(new Action1<String>() {
    public void call(String s) {
      Log(s);
    }
  })
```

---

线程调度的使用就是这样

## 原理

不会