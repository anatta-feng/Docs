---
title: NDK 开发初体验
date: 2017-12-12 17:05:25
categories: [笔记]
tags: [NDK]
---
> 记录自己今天踩到的坑
>
> CMake 环境

# Java

* Java 中 loadLibrary 的时候，传入的 library name 是在 CMake 中定义的 module name。
  * 我就因为名字没写对，报了`UnsatisfiedLinkError`错误

# CMake

## add_library

```Cmake
add_library(<name> <SHARED|STATIC|MODULE|UNKNOWN> IMPORTED
            [GLOBAL])
```

每次要新建一个模块的时候，就要这样。

## target_link_libraries

每个模块有依赖的时候，都要去声明

格式：

```cmake
target_link_libraries(<target>
                    <PRIVATE|PUBLIC|INTERFACE> <lib> ...
                    [<PRIVATE|PUBLIC|INTERFACE> <lib> ... ] ...])
```

target 就是被添加依赖的模块，下面就是要添加的依赖

# 添加第三方的源码

##首先需要添加源码所在的路径

```cmake
include_directories()
```

## 然后调用`add_library` 将第三方源码作为一个模块添加进去

比如 `AVI_LIB`

```cmake
include_directories(
    /Users/fxc/Documents/lib/transcode-1.1.7/avilib
)
add_library( avi-lib
             SHARED
             /Users/fxc/Documents/lib/transcode-1.1.7/avilib/avilib.c
             /Users/fxc/Documents/lib/transcode-1.1.7/avilib/platform_posix.c)
```

#添加第三方 so 依赖



#设置变量

```cmake
set(<variable> <value>
    [[CACHE <type> <docstring> [FORCE]] | PARENT_SCOPE])
```

