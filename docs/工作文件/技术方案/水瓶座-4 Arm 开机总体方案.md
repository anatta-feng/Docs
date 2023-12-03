# 背景

水瓶座-4 发送卡与 Android 板卡是分开的，而且 Android 卡需要可以控制发送卡。因此需要在 Android 中运行 XServer 以及 XServer 的依赖，并且需要对 XServer 提供一些平台能力的封装。

# 功能要求

1. 硬件正常的情况下，始终可以正常点亮屏幕

# 模块

## XServer

LED 控制协议实现

### 运行前提条件

- Arm 工具包正常提供
- 发送卡 MCU 以及 FPGA 版本号与 XServer 版本号匹配

### Arm 工具包

提供 MCU、FPGA 升级能力，以及 VByOne 接口信息封装等能力

#### 运行前提条件

- 与发送卡通信的网口已经正常启动，可以进行网络通信
- MCU 与 FPGA 存活

# 开机流程

## 正常情况

![[KT60-Normal.png]]

## Arm 工具包补充流程

### 升级失败

![[KT60-Arm-Error.png]]

## 备份流程

该流程需要在升级措施之前

![[KT60-Arm-Backup.png]]