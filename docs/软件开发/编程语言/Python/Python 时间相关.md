# 获取当前时间戳

```python
import time
time.time()
```

# 时间字符串 --> 时间戳

```python
timestring = '2016-12-21 10:22:56'
print time.mktime(time.strptime(timestring, '%Y-%m-%d %H:%M:%S')) # 1482286976.0
```

* time.mktime() 与 time.localtime() 互为还原函数。
* time.mktime(timetuple) ：将时间元组转换成时间戳
* time.localtime([timestamp])：将时间戳转会为时间元组

# 时间戳 --> 时间字符串

## time 模块

```python
timestamp = time.time()
timestruct = time.localtime(timestamp)
print time.strftime('%Y-%m-%d %H:%M:%S', timestruct) # 2016-12-22 10:49:57
```

##  datetime 模块

```python
import datetime
timestamp = 1482374997.55
datetime_struct = datetime.datetime.fromtimestamp(timestamp)
print datetime_struct.strftime('%Y-%m-%d %H:%M:%S') # 2016-12-22 10:49:57

datetime_struct = datetime.datetime.utcfromtimestamp(timestamp)
print datetime_struct.strftime('%Y-%m-%d %H:%M:%S') # 2016-12-22 02:49:57
```

* `fromtimestamp(timestamp[, tz])`：将时间戳转为当地的时间元组
* `utcfromtimestamp(*timestamp*)`：将时间戳转为UTC的时间元组。以北京为例：utc时间比北京当地时间少8个小时。

# python根据日期返回星期几的方法

```python
print(datetime.datetime.now().weekday())
```

