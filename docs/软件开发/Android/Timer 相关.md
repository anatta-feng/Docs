---
title: Timer 相关
date: 2016-11-22 17:05:25
categories: [笔记]
tags: [Android]
---

# 关于判断计时器是否结束

> 今天遇到一个需求，功能的实现需要计数器，并利用计时器清零计数器，但在实现过程中遇到一个棘手的问题：每次点击计时，并且启动一个计时器，时间到了以后清零计数器，但是因为每次都会启动一个计时器，在第一个计时器结束后就会迎来所有的计时器到期，计数器一直为零。
>
> 请教同事后同事提醒用线程标志位，但是标志位貌似是线程池的问题，我还不太了解，于是想出一个曲线救国的方法，如下

<!-- more -->

```java
class MainActivity extends Activity {
	Timer timer;
	public boolean onKeyDown(int keyCode, KeyEvent event0 {
    	// 逻辑代码
    	if(timer == null) {
        	timer = new Timer();
        	timer.schedule(new TimerTask() {
            	@Override
            	public void run() {
                	//逻辑代码
                	timer = null;		// 设置 tmer == null，这样在计时										   器结束前都不会再启动新的计数器
            	}
        	}, 1000);
    	}
    	return true;
	}
}
```

------

对于标志位的问题，研究后再写。

