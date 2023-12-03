---
title: Java 每天固定时刻执行特定代码
date: 2017-6-9 17:05:25
categories: [笔记]
tags: [编程, Java]
---

遇到一个功能，每天需要去爬取数据存储到数据库中，用 Java 实现

```java
public void timerTask() {
  final long DAY_SPAN = 1000 * 60 * 60 * 24;
  Calendar c = Calendar.getInstance();
  // 设置需要执行任务的时间点，现在设置的是每天凌晨五点
  c.set(Calendar.HOUR_OF_DAY, 5);
  c.set(Calendar.MINUTE, 0);
  c.set(Calendar.SECOND, 0);
  
  Date mDate = c.getTime();
  
  if(mDate.before(new Date())) {
    mDate = addDay(mData, 1);
  }
  Task mTask = new Task();
  mTimer.scheduleAtFixedRate(mTask, mDate, DAY_SPAN);
}

private Date addDay(Date date, int i) {
  Calendar c = Calendar.getInstance();
  c.setTime(mDate);
  c.add(Calendar.DAY_OF_MONTH, i);
  return c.getTime();
}
```

这个方法可以放到`ServletContextListener`中调用

就是不知道 Timer 的线程能不能保留那么久；

还有更好的方法，就是用框架，`quartz`