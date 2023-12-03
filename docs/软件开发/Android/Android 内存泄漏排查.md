---
title: Android 内存泄漏排查
date: 2017-09-8 12:15:00
categories: [笔记] 
tags: [Android, 内存泄漏]
---

> 排查步骤：
>
> 1. 抓取 Java 堆信息，有三种方法
>    1. SDK 的 DDMS 来抓取
>
>    2. 用 Android Studio 集成的 DDMS 抓取
>
>    3. 通过 shell 抓取
>
>       > shell 抓取是通过 ActivityManager 来抓取的
>       >
>       > 命令：`am dumpheap [pid] [filePath]` ，最好用 root 权限去抓取，注意，有 root 权限抓取后要对生成的文件改权限。
>       >
>       > 然后拉取到电脑上
>
> 2. 打开 hprof 文件有很多方法，我常用的是 MAT 和 Android Studio
>
>    1. 如果是 Android Studio 的话，抓取出来的 hprof 文件就可以直接用
>    2. 如果是用 MAT 来分析，那就需要先将 hprof 转码，用 SDK 提供的工具可以转码。`hprof-conv [inputFile] [outputFile]`
>
> 3. 开始分析

# 抓取 Java 堆信息

有三种方法

1. SDK 的 DDMS 来抓取

2. 用 Android Studio 集成的 DDMS 抓取

3. 通过 shell 抓取

   > shell 抓取是通过 ActivityManager 来抓取的
   >
   > 命令：`am dumpheap [pid] [filePath]` ，最好用 root 权限去抓取，注意，有 root 权限抓取后要对生成的文件改权限。
   >
   > 然后拉取到电脑上

# 打开 Hprof 文件

打开 hprof 文件有很多方法，我常用的是 MAT 和 Android Studio

1. 如果是 Android Studio 的话，抓取出来的 hprof 文件就可以直接用
2. 如果是用 MAT 来分析，那就需要先将 hprof 转码，用 SDK 提供的工具可以转码。`hprof-conv [inputFile] [outputFile]`

# 分析

