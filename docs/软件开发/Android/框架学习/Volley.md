# Volley 源码 

## 1. 新建请求队列

> Volley.newRequestQueue(Context context, HttpStack stack, int maxDiskCacheBytes)

新建缓存 cacheDir

首先判断传入的 HttpStack 是否为空，如果是空就会根据 Android Sdk 版本来创建相应的 Stack

> HttpStack 是负责网络请求的类，有相应的子类，内部分别使用 HttpURLConnection 和 HttpClient 来进行网络请求
>
> 详情参见 [Android访问网络，使用HttpURLConnection还是HttpClient？](http://blog.csdn.net/guolin_blog/article/details/12452307) 

注： String userAgent 目前不知道有什么用，留待稍后

---

接下来就会实例化一个 RequestQueue，并且调用他的 start() 方法

---

## 2. RequestQueue 的 start() 方法

在 start() 方法里首先会 new 一个 CacheDispatcher 并且调用他的 start() 方法

接下来会在一个 for 循环里 new 出相应数量的 NetworkDispatcher 并且调用他的 start() 方法，这个数量是由在实例化 RequestQueue 时新建的一个数组的长度决定的，而这个数组的长度一般默认为 4

CacheDispatcher 和 NetworkDispatcher 均是继承自 Thread，所以在这一步后后台便会一直有五个线程在运行，其中一个是缓存线程(CacheDispatcher)，其他四个是网络请求线程 (NetworkDispatcher)。

start() 方法到这里就结束了，接下来就是新建一个请求并且加入队列，调用的是 add() 方法。

---

## 3. RequestQueue 的 add() 方法

首先对传进来的 request 进行了一定的初始化

然后会判断这个请求是否需要缓存

```java
if (!request.shouldCache()) {
    mNetworkQueue.add(request);
    return request;
}
```

如果不需要缓存则会添加到队列中后直接 return

---

接下来会进行缓存操作，首先获取到缓存时候用的 key 值

```java
String cacheKey = request.getCacheKey();
```

```java
/**
接下来会去比对这个 key 值有没有，如果有的话代表有缓存,
```

> 加入缓存队列前的的逻辑有点没看懂

默认有缓存，所以队列最终会加入缓存队列```mCacheQueue.ad(request);```

接下来看看缓存线程的 run() 方法.

---

## 3. 缓存线程 CacheDispatcher 的 run 方法

在 run() 方法中有一个死循环，会一直尝试去处理请求，如果没有请求就会报异常，在异常处理中只是跳出本次循环

当有请求传进来的时候，首先会用这个请求获取 CacheKey ```request.getCacheKey()``` 如果没有缓存则会直接加入网络请求线程```add()```中。

如果有缓存则会判断缓存是否过期，如果过期了也直接加入网络请求线程里。

> 在 VOD 改写的 Volley 里直接将判断缓存是否过期还有是否需要更新缓存的判断取消了，每次都强制读取缓存，强制刷新缓存

如果没有过期则会读取出缓存，解析

``` new NetworkResponse(entry.data,entry.responseHeaders));```

然后再根据是否需要更新缓存来发起一条优先度较低的网络请求```put()```来更新缓存。

> BlockingQueue 的 add() 方法与 put() 方法的区别
>
> * add() 方法
>   * Inserts the specified element into this queue if it is possible to do so immediately without violating capacity restrictions, returning
>   * 个人理解是只要在能力范围内，就会立即执行传进来的请求
> * put() 方法
>   * Inserts the specified element into this queue, waiting if necessary for space to become available.
>   * 个人理解：请求传进来后如果当前没有可用的空间，或者队列在忙，就去等待，直到队列有了空闲时间才会执行这个
> * 这两个方法的区别应该就是优先级的区别

---

## 4. 网络请求线程 NetworkDispatcher 中 run() 方法

同样的死循环

调用底层网络连接逻辑，然后解析，然后一层层回调给 Request