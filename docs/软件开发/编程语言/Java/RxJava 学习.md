---
title: RxJava 学习
date: 2017-4-25 15:20
categories:
  - 笔记
tags:
  - 代码
---
# RxJava 学习

> RxJava 核心是观察者模式，并且有一点自己的处理改动。
>
> RxJava 最大的特点是将一些回调或者迷之缩进的代码，改变成一串的线性逻辑，这样代码理解起来会特别顺。

## 基本概念

> RxJava 的核心概念是依托观察者模式的，非常类似

* Observable 
  * 被观察者，被订阅者，类似生活中报社的存在
* Observer
  * 观察者、订阅者。类似生活中读者的角色
* Subject
  * s
* Subscriber
  * 订阅者、**Observer** 的补充，增加了 `unsubscribe()` 、`onStart()` 方法。
    * `onStart()` 方法是最开始调用的，可以用于一些初始化的操作
    * `unsubscribe()` 是用来取消订阅的方法，如果不调用这个方法，那会存在内存泄漏的风险。
* Subscription
  * **Observable** 调用`subscribe()` 方法返回的对象
* Action0
  * RxJava 中的一个接口，只有一个无参无返回的的 call 方法。还有类似的 Action1 至 Action9， 区别是相应的 `call()` 方法的有了相应数量的参数
* Func0
  * 与 Action0 非常相似，区别是有了返回值



## 基本用法

* Observable 的创建

  * `create()` ，最基础的创建方式

    * ```java
      normalObservable = Observable.create(new Observable.onSubscribe<String>() {
       public void call(Subscriber<? super String> subscriber) {
         subscriber.onNext("create1");	//发送一个“create1”的 String
         subscriber.onNext("create2");	//发送一个“create2”的 String
         subscriber.onCompleted();		//所有的消息发送完成，create创建方式需要手动调用onCompleted
       } 
      })
      ```

  * `just()` 

    * 创建一个Observable 并自动调用 `onNext()` 用来发送数据

    * ```java
      //逐次发送“just1”、“just2”两条String
      justObservable = Observable.just("just1", "just2");
      ```

  * `from()` 遍历集合，发送每个 Item

    * ```java
      ArrayList<String> list = new ArrayList<>;
      for(int i = 0; i++; i <10) {
        list.add("a" + i);
      }
      fromObservable = Observable.from(list);
      ```

    * ​

  * `defer()` ，有观察者订阅时才创建 **Observable**，并且为每个观察者创建一个新的 **Observable** 

    * ```java
      deferObservable = Observable.defer(new Func0<Observable<String>() {
        public Observable<String> call() {
          return Observable.just("deferObservable");
        }
      });
      ```

  * `interval()`，创建一个按固定时间间隔发送整数序列的 **Observable**，可用作定时器

    * ```java
      //每隔一秒发送一次
      intervalObservable = Observable.interval(1, TimeUnit.SECONDS);
      ```

  * `range()`，创建一个发射特定整数序列的 **Observable**，第一个参数为起始值，第二个为发送的个数，如果为零则不发送，负数则抛异常

    * ```java
      //将依次发送 4、5、6、7、8
      rangeObservable = Observable.range(4, 5);
      ```

  * `timer()` 创建一个 **Observable**，它在给定的延迟后发送一个值

    * ```java
      //一天后发送
      timeObservable = Observable.timer(1, TimeUnit.DAYS);
      ```

  * `repeat()`,创建一个重复发送特定数据的 **Observable**

    * ```java
      repeatObservable = Observable.just("sub").repeat(3);	//重复发送3次
      ```

* Observer 的创建

  * ```java
    mObserver = new Observer<String>() {
      public void onCompleted() {
        Log("onCompleted");
      }
      public void onError(Throwable e) {
        
      }
      public void onNext(String s) {
        Log(s);
      }
    };
    ```

## 关联 Observable 与 Observer

创建好 Observable 与 Observer 之后，将两个关联起来就可以开始工作了

```java
justObservable.subscribe(mObserver);
```

mObserver 的 `onNext()` 方法将会依次收到“just1”、“just2”。如果不需要在意如果数据出错、或着数据是否传送完成，就不需要 **Observer** 的`onCompleted()` 和`onError()` 方法，可以使用`Action1`，`subscribe()` 支持使用 `Action`为参数传入，RxJava会调用它的`call()`方法来接收数据：

```java
justObservable.subscribe(new Action1<String>() {
  public void call(String s) {
    Log(s);
  }
});
```

---

至此，RxJava的基础使用结束了

然而实际上这样的用法并没有什么用

RxJava 的默认规则是事件的产生和消费都在同一个线程，所以如果只这样用的话，相当于实现了一个同步的回调，然而使用回调的本身目的就是`后台处理，前台回调`。所以异步是RxJava的灵魂。

要实现异步，就需要RxJava的另一个概念 `Scheduler`

