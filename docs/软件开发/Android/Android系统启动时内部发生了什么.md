---
title: Android 系统启动时内部发生了什么
date: 2017-1-24 12:05:25
categories: [笔记]
tags: [Android, 内核]
---

# Loader 层

* Boot ROM：是手机在关机状态下长按电源键开机，引导芯片会从固话在 ROM 里预设代码开始执行，然后加载引导程序到 RAM。
* Boot Loader：这是启动 Android 前的引导程序，主要是检查 RAM，初始化硬件参数等功能。

这些事情发生在进入 Android 系统之前

# Kernel 层

Kernel 层是 Android 的内核层，从这里才开始进入 Android 系统。

* 启动 Kernel 的 swapper 进程（pid = 0）：该进程又称为 idle 进程，系统初始化过程 Kernel 从无到有创建的第一个进程，用于初始化进程管理，内存管理，加载 Display，Camera Diver，Binder Driver 等相关工作。
* 启动 kthreadd 进程（pid = 2）：![](https://ww3.sinaimg.cn/large/006tKfTcly1fc1k6naoo5j30pi02ijrt.jpg) 这是 Linux 系统的内核进程，会创建内核工作线程 kworder，软中断线程 ksoftirqd，thermal 等内核守护线程。**kthreadd进程是所有内核进程的鼻祖。**

# Native 层

这里的 Native 层主要包括 `/init` 孵化来的用户空间的守护进程、HAL 层以及开机动画等。启动 `/init` 进程（pid = 1），是Linux 系统的用户进程，**/init 进程是所有用户进程的鼻祖**。

* /init 进程会孵化出 ueventd、logd、healthd、installd、adbd、lmkd 等用户进程；
* /init 进程还启动 servicemanager（binder 服务管家）、bootanim（开机动画）等重要服务；
* /init 进程孵化出 Zygote 进程，Zygote 进程是 Android 系统的第一个 Java 进程，**Zygote 是所有 Java 进程的父进程**，Zygote 进程本身是由 /init 进程孵化而来的。


# Framework 层

* Zygote 进程，是由 /init 进程通过解析 init.rc 文件后 fork 生成的，Zygote 进程主要包含：
  * 加载 ZygoteInit 类，注册 Zygote Socket 服务端套接字；
  * 加载虚拟机；
  * preloadClasses；
  * preloadResouces
* System Server 进程，是由 Zygote 进程 fork 而来，**System Server 是Zygote 孵化的第一个进程**， System Server 负责启动和管理整个 Java Framework，包含 ActivityManager，PowerManager 等服务
* Media Server 进程，是由 /init 进程 fork 而来，负责启动和管理整个 C++ framework，包含 AudioFlinger，Camera Service，等服务

# App 层

* Zygote 进程孵化出第一个 App 进程是 Launcher，这是用户看到的桌面 App；
* Zygote 进程还会创建 Browser，Phone，Email 等 App 进程，每个 App 至少运行在一个进程上。
* 所有的 App 进程都是由 Zygote 进程 fork 生成的。



> 以上内容参考（也可以说照搬）：[Gityuan](http://gityuan.com/android/)



是时候研究系统了，以前总是畏手畏脚的，干脆放开手脚，研究过程中不会了就去学，学完了回来继续研究，就这样。