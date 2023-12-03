#Android #AOSP

# 目录

![[android_13_dir.png]]

- art：Android Runtime，一种App运行模式，区别于传统的Dalvik虚拟机，旨在提高Android系统的流畅性
- bionic：基础C库源代码，Android改造的C/C++库
- bootable：Android程序启动导引，适合各种bootloader的通用代码，包括一个recovery目
- build：存放系统编译规则及generic等基础开发包配置
- cts： Android兼容性测试套件标准
- dalvik：Android Dalvik虚拟机相关内容
- developers：Android开发者参考文档
- development： Android应用开发基础设施相关
- device：Android支持的各种设备及相关配置
- external：Android中使用的外部开源库
- frameworks：应用程序框架，Android系统核心部分，由Java和C++编写
- hardware：硬件适配接口
- kernel：Linux Kernel，不过Android默认不提供，需要单独下载，只有一个tests目录
- libcore：Android Java核心类库
- libnativehelper：Android动态库，实现JNI库的基础
- packages：应用程序包
- pdk：Plug Development Kit 的缩写，本地开发套件
- platform_testing：Android平台测试程序
- prebuilts：x86和arm架构下预编译的一些资源
- sdk：Android的Java层sdk
- system：Android底层文件系统库、应用和组件
- test：Android Vendor测试框架
- toolchain：Android工具链文件
- tools：Android工具文件

# 目录解析

## build

存放系统编译规则及generic等基础开发包配置，我们如果需要进行系统开发或者只是想改动系统源码然后编译一下系统，是需要对这块了解的。目录如下：
![[aosp_13_build_dir.png]]
简单介绍如下：

| 目录名 |	介绍 |
| --- | --- |
|blueprint	|输入为.bp文件。输出为.ninja文件|
| core | 核心的编译规则 makefile |
| kati | kati is an experimental GNU make clone |
| make | 以前的老的make系统 |
| soong | 新的Build系统 |
| target | AOSP自带的Target(模拟器)的一些makefile |
| tools | 编译中使用的shell及python写的工具脚本 |
| build/envsetup.sh | 编译初始化脚本 |

## framework

Android 核心应用都在这个 framework 中，我们经常说的 framework 层的开发就是基于这块。

![[android_13_aosp_dir.png]]

简单介绍如下：

| 目录名 | 介绍 |
| --- | --- |
| framework/av/ | 多媒体相关的native层源码目录 |
| framework/base/ | 一些基础库代码，各种解析类、工具类都在这个里面 |
| framework/compile/ | 编译相关的内容 |
| framework/ex/ | ex文件解析器 |
| framework/minikin/ | Android原生字体 |
| framework/ml/ | 机器学习 |
| framework/multidex/ | multi dex Loader |
| framework/native/ | power、surface、input、binder等服务的native层实现源码目录 |
| framework/opt/	 | 一些基础软件，如：日历、网络、蓝牙 |
| framework/rs/ | Render Script 可创建3D接口 |
| framework/wilhelm/ | OpenSL ES/OpenMAX AL的audio |

