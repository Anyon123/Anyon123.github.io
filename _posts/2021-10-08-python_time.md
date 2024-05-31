---
title: python时间转换
tags: python develop
author: Anyon
aside:
  toc: true
sidebar:
  nav: layouts
---



##### 常用方法介绍

>`datetime` 包
>
>- `datetime.datetime.strptime(string, format)` 类方法，作用是根据指定的格式将字符串转化为`datetime.datetime`实例对象（format一般可用: %Y-%m-%d %H:%M:%S）
>- `datetime.datetime.strftime(format)` 实例方法，作用是根据指定的格式将`datetime.datetime`实例对象转化为时间字符串
>- `datetime.datetime.timestamp()` 实例方法，作用是将`datetime.datetime`实例对象转化为时间戳
>- `datetime.fromtimestamp(timestamp, tz=None)` 类方法，作用是将时间戳转化为`datetime.datetime`实例对象
>- `datetime.datetime.combine(date, time, tzinfo=True)` 类方法，作用是将`datetime.date()`实例对象和`datetime.time()`实例对象结合起来
>
>
>
>`time` 包
>
>- `time.strptime(string, format)` 类方法，作用是根据指定的格式将时间字符串转化成`time.struct_time`对象
>- `time.strftime(format,  string)` 类方法， 作用是根据指定格式将`time.struct_time`对象转化成时间字符串
>- `time.localtime(timestamp)` 类方法，作用是将时间戳转换成本地时间的`time.struct_time`对象，若要转换成UTC时间，则使用`time.gtime()`
>- `time.mktime(t)` 类方法，将`time.struct_time`对象转换成时间戳
>
>







##### 1 求当天日期

```
from datetime import date

res = date.today()  # date格式
print(res)
```

##### 2 求当天零点时间戳

```
from datetime import date, datetime
from time import mktime

res = datetime.combine(date.today(), datetime.min.time())  # datetime格式
print(res)
print(res.timestamp())  # datetime转timestamp
res = int(mktime(date.today().timetuple()))  # timestamp格式
print(res)
print(datetime.fromtimestamp(res))
```

