---
title: SimpleAlarmDemo
date: 2016-9-27 15:20
categories:
  - 笔记
tags:
  - Android
---

# 这是一个简易的闹钟 Demo

[Demo地址](https://github.com/fxc0719/SimpleAlarmDemo.git)

目前仅实现到达指定时间弹出 Dialog 提醒

## 实现步骤

- 定义好两个 Activity，一个作为主界面，一个作为提醒的 Dialog

- 对 Button 设置监听器，点击后调用 TimePickerDialog，显示出选择时间的 Dialog

- 对 TimePickerDialog 设置监听器，重写 onTimeSet 方法，

- 在 onTimeSet 方法中将 calendar 时间设置为当前时间，其实不用设置也可以，不过使用变量前先初始化是一个良好的编程习惯。

- 新建 Intent，并用 PendingIntent 包装 Intent。

- 实例化一个 AlarmManager，并且调用系统服务：ALARM_SERVICE

- AlarmManager 调用 set 方法设置闹钟，传入 PendingIntent

  <!-- more -->


---

## 现存疑点：

如何在一个 Dialog 消失的时候弹出一个 SnackBars？ 一个 Dialog 是一个 Activity，SnackBars 需要传入一个 View，Dialog 消失的时候 Acticity 会死亡，SnackBars 的 View 从何而来？

> 初步想法，两个 Activity 通信，Dialog 死亡时向 MainAvtivity 传递一个信息，MainActivity 收到信息后弹出一个 SnackBars，具体实现有待解决



---

# 9.27 — 尝试添加贪睡功能失败

### 之前思路

> 新建 MyAlarmManager 类，继承 AlarmManager，然后通过实现 Serializable 接口，通过 Intent 在 Activity 间传递 Alarmanager，然后在 Dialog 中通过点击时间取消闹钟

但是在继承 AlarmManager 时出错，提示构建构造方法，但是新建无参构造方法后调用 super() 仍然出错，查看文档发现 AlarmManager 并没有构造方法。失败告终

### 新思路

> 在第一次创建闹钟时创建一个单次闹钟，然后在 Dialog 中通过点击事件创建下次闹钟，曲线救国