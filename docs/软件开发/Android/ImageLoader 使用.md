---
title: ImageLoader 使用
date: 2017-05-3 12:15:00
categories: [笔记] 
tags: [Android]
---

# ImageLoader 使用

先学会怎么用，然后再从源码角度分析。

## 初始化

使用任何对象第一步肯定是初始化。

### 配置

首先需要初始化加载图片需要用到的配置。比如：

* 线程池数量
* 是否缓存
* 缓存大小
* 等等...

上代码

ImageLoaderConfiguration

```java
ImageLoaderConfiguration mConfig = new ImageLoaderConfiguration
  .Builder(ctx)
  //
  .memoryCacheExtraOptions(width, height)
  //
  .diskCacheExtraOptions(width, height, new BitmapProcessor)
  //线程池数量
  .threadPoolSize(size)
  //线程优先级
  .threadPriority(Thread.NORM_PRIORITY)
  //
  .denyCacheImageMultipleSizeInMemory()
  //
  .memoryCache(new UsingFreqLimitedMemoryCache(size))
  //内存缓存区大小
  . memoryCacheSize(size)
  //
  .tasksProcessingOrder(QueueProcessingType.LIFO)
  //
  .diskCache(new UnlimiteDiskCache(cacheDir))
  //
  .diskCacheFileCount(count)
   //磁盘缓存大小
  .diskCacheSize(size)
  //磁盘缓存文件文件名
  .diskCacheFileNameGenerator(new Md5FileNameGenerator())
  //
  .defaultDisplayImageOptions(DisplayImageOptions.createSimple())
  //
  .imageDownloader(new BaseImageDownloader(ctx, , ))
  //
  .writeDebuglogs()
  //
  .build();
```

### 初始化 ImageLoader

```java
ImageLoader.getInstance().init(mConfig);
```

### 初始化 Display Options

---

Display Options是图片的显示配置

```java
DisplayImageOptions mOptions = new DisplayImageOptions.Builder()
  //
  .showImageOnLoading(R.drawable.ic_stub)
  //
  .showImageForEmptyUri(R.drawable.ic_empty)
  //
  .showImageOnFail(R.drawable.ic.error)
  //
  .resetViewBeforeLoading(false)
  //
  .delayBeforeLoading(1000)
  //
  .cacheInMemory(false)
  //
  .cacheOnDisk(false)
  //
  .preProcessor(...)
  //
  .postProcessor(...)
  //
  .extraForDownloader(...)
  //
  .considerExifParams(false)
  //
  .imageScaleType(ImageScaleType.IN_SAMPLE_POWER_OF_2)
  //
  .bitmapConfig(Bitmap.Config.ARGB_8888)
  //
  .decodingOptions(...)
  //
  .displayer(new SimpleBitmapDisplayer())
  //
  .handler(new Handler())
  //
  .build();
```

#### 使用 Display Options

```java
ImageLoader.getInstance().displayImage(url, view, mOptions);
```

## 加载图片

### loadImage()

`loadImage()`提供五个参数

* uri
  * 图片链接
* ImageSize
  * 图片显示大小
* DisplayImageOptions
  * 图片显示方式
* ImageLoadingListener
  * 加载图片生命周期监听
* ImageLoadingProgressListener
  * 通俗的说就是加载进度

可以有选择的带参数来实现

---

### displayImage()

`displayImage()`方法需要最多六个参数

* uri
  * 图片链接
* ImageAware
  * 暂时不明白
* DisplayImageOptions
  * 图片显示参数
* ImageSize
  * 图片大小
* ImageLoadingListener
  * 图片加载生命周期监听器
* ImageLoadingProgressListener
  * 加载进度