### **4.7 datetime模块**

####     **4.7.1 获取当前日期和时间**

```python
from datetime import datetime

print(datetime.now())

'''
结果:2018-12-04 21:07:48.734886
'''
```

​    **4.7.2 获取指定日期和时间**

要指定某个日期和时间，我们直接用参数构造一个`datetime`：

```python
from datetime import datetime

dt = datetime(2018,5,20,13,14)
print(dt)

'''
结果:2018-05-20 13:14:00
'''
```

​    **4.7.3 datetime与时间戳的转换**

```python
from datetime import datetime

dt = datetime.now()
new_timestamp = dt.timestamp()
print(new_timestamp)

'''
结果:1543931750.415896
'''

import time
from datetime import datetime

new_timestamp = time.time()
print(datetime.fromtimestamp(new_timestamp))
```

####     **4.7.4 str与datetime的转换**

​    很多时候，用户输入的日期和时间是字符串，要处理日期和时间，首先必须把str转换为datetime。转换方法是通过`datetime.strptime()`实现，需要一个日期和时间的格式化字符串：

~~~python
from datetime import datetime

t = datetime.strptime('2018-4-1 00:00','%Y-%m-%d %H:%M')
print(t)
'''
结果: 2018-04-01 00:00:00
'''

# 如果已经有了datetime对象，要把它格式化为字符串显示给用户，就需要转换为str，转换方法是通过`strftime()`实现的，同样# 需要一个日期和时间的格式化字符串：

```
from datetime import datetime
now = datetime.now()
print(now.strftime('%a, %b %d %H:%M'))
Mon, May 05 16:28
```


~~~

###     **4.7.5 datetime加减**

对日期和时间进行加减实际上就是把datetime往后或往前计算，得到新的datetime。加减可以直接用`+`和`-`运算符，不过需要导入`timedelta`这个类：

```python
from datetime import datetime, timedelta
now = datetime.now()
now
datetime.datetime(2015, 5, 18, 16, 57, 3, 540997)
now + timedelta(hours=10)
datetime.datetime(2015, 5, 19, 2, 57, 3, 540997)
now - timedelta(days=1)
datetime.datetime(2015, 5, 17, 16, 57, 3, 540997)
now + timedelta(days=2, hours=12)
datetime.datetime(2015, 5, 21, 4, 57, 3, 540997)
```

可见，使用`timedelta`你可以很容易地算出前几天和后几天的时刻。

####     **4.7.6 指定datetime时间**

```python
current_time = datetime.datetime.now()
print(current_time.replace(year=1977))  # 直接调整到1977年
print(current_time.replace(month=1))  # 直接调整到1月份
print(current_time.replace(year=1989,month=4,day=25))  # 1989-04-25 18:49:05.898601
```

