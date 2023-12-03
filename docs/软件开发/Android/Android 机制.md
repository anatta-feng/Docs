# KeyEvent.Callback
可以用来给感触不到按键事件的类来传递按键事件
实现此接口，然后在可以接收到按键事件的类中的dispatchKeyEvent方法中调用`event.dispatch(this)`将实现接口的类传递进去即可。

```kotlin
class Fragment : KeyEvent.Callback {
  fun dispatchKeyEvent(*event*: KeyEvent) {
    // this 指的是 Callback
    event.dispatch(this)
  }
}
```
# 消息机制
Android 消息机制是由 handler， MessageQueue, Looper 组成的。
handler 拿到 message 后先判断 message 的 Callback（Runnbale）是否为 null（是否调用的 Handler post 方法），如果不为 null，则直接 run，如果为 null 则传递给handleMessage方法。
# 消息是怎么从子线程到主线程的
主线程一直在干两件事，loop，还有 loop 到结果后去执行结果。
在子线程 sendMessage 的时候，是把 Message 传递给 Looper 的 MessageQueue 了，接下来因为 Looper 一直在主线程取数据，取到后也在主线程执行。所以消息从子线程到了主线程。
# PackageManager
# 获取安装的应用列表
首先介绍返回的 Bean：**ApplicationInfo**
* 包名：packageName
* 应用名称： getLable

#踩坑