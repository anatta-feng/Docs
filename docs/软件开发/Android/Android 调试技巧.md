# 广播相关

## 获取广播事件

利用命令  `dumpsys activity broadcasts`

或者 `dumpsys activity broadcasts history`

# 进程相关

## 查看进程是被谁拉起的

查看 logcat 会发现进程启动的时候会打印一行 log

```verilog
06-08 06:10:16.921 I/ActivityManager( 3035): Start proc 4299:com.xxx/1000 for activity com.xxx/.GuideActivity
```

这行 log 什么意思呢？看看代码：

```java
checkTime(startTime, "startProcess: building log message");
StringBuilder buf = mStringBuilder;
buf.setLength(0);
buf.append("Start proc ");
buf.append(startResult.pid);
buf.append(':');
buf.append(app.processName);
buf.append('/');
UserHandle.formatUid(buf, uid);
if (!isActivityProcess) {
		buf.append(" [");
  	buf.append(entryPoint);
  	buf.append("]");
}
buf.append(" for ");
buf.append(hostingType);
if (hostingNameStr != null) {
  	buf.append(" ");
  	buf.append(hostingNameStr);
}
Slog.i(TAG, buf.toString());
```

还有这行 log

```log
START u0 {cmp=com.toner.memdemo/.MainActivity (has extras)} from uid 10054
```
