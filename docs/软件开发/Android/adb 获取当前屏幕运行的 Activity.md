# adb 获取当前屏幕运行的 Activity

> 有时候我们反编译学习源码，但是代码可能混淆了，导致阅读非常困难，这时候就需要一些辅助方法帮我们快速定位到我们想要学习的代码

## 过滤日志

```adb shell```

```logcat | grep ActivityManager```

这样会过滤出 TAG 为 ActivityManager 的 logcat

```verilog
D/ActivityManager(  936): AP_PROF:AppLaunch_LaunchTime:com.example.chenzongwen.myapplication/.LeakActivity:155:20438126
```

像实例代码中有```AP_PROF:AppLaunch_LaunchTime:```头的 Logcat 后面跟的就是应用包名加 Activity 名了