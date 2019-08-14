### **time模块**

​    time翻译过来就是时间,这个模块是与时间相关的模块，那么言外之意，如果我们在工作中遇到了对时间的需求（比如获取当前时间，获取时间戳等等）就要先想到time模块。

time模块中对于时间可以分成三种形式:

1. 时间戳: 通常的叫法,时间戳表示的是格林尼治时间是从1970年1月1日00:00:00开始按秒计算的偏移量。这个是实时变化的。我们运行“type(time.time())”，返回的是float类型
2. 格式化字符串时间: 格式化的时间字符串(Format String)： ‘1999-12-06’

```python
python中时间日期格式化符号： 
%y 两位数的年份表示（00-99） 
%Y 四位数的年份表示（000-9999） 
%m 月份（01-12） 
%d 月内中的一天（0-31） 
%H 24小时制小时数（0-23） 
%I 12小时制小时数（01-12） 
%M 分钟数（00=59） 
%S 秒（00-59） 
%a 本地简化星期名称 
%A 本地完整星期名称 
%b 本地简化的月份名称 
%B 本地完整的月份名称 
%c 本地相应的日期表示和时间表示 
%j 年内的一天（001-366） 
%p 本地A.M.或P.M.的等价符 
%U 一年中的星期数（00-53）星期天为星期的开始 
%w 星期（0-6），星期天为星期的开始 
%W 一年中的星期数（00-53）星期一为星期的开始 
%x 本地相应的日期表示 
%X 本地相应的时间表示 
%Z 当前时区的名称 
%% %号本身
```

**上面这些在课上简单的练习几个就行。**

3. 结构化时间:元组(struct_time) struct_time元组共有9个元素共九个元素:(年，月，日，时，分，秒，一年中第几周，一年中第几天等）

首先，我们先导入time模块，来认识一下python中表示时间的几种格式：

```python
#导入时间模块
>>>import time

#时间戳
>>>time.time()
1500875844.800804

#时间字符串
>>>time.strftime("%Y-%m-%d %X")
'2017-07-24 13:54:37'
>>>time.strftime("%Y-%m-%d %H-%M-%S")
'2017-07-24 13-55-04'

#时间元组:localtime将一个时间戳转换为当前时区的struct_time
time.localtime()
time.struct_time(tm_year=2017, tm_mon=7, tm_mday=24,
　　　　　　　　　　tm_hour=13, tm_min=59, tm_sec=37, 
                 tm_wday=0, tm_yday=205, tm_isdst=0)
```

小结：时间戳是计算机能够识别的时间；时间字符串是人能够看懂的时间；元组则是用来操作时间的

时间模块我们就是学会如何获取当前的时间，以及三种时间之间的转化就行了。

**时间格式转换：**

![img](http://crm.pythonav.com/media/uploads/2019/04/17/IMAGE.PNG)

```python
#时间戳-->结构化时间
#time.gmtime(时间戳)    #UTC时间，与英国伦敦当地时间一致
#time.localtime(时间戳) #当地时间。例如我们现在在北京执行这个方法：与UTC时间相差8小时，UTC时间+8小时 = 北京时间 
>>>time.gmtime(1500000000)
time.struct_time(tm_year=2017, tm_mon=7, tm_mday=14, tm_hour=2, tm_min=40, tm_sec=0, tm_wday=4, tm_yday=195, tm_isdst=0)
>>>time.localtime(1500000000)
time.struct_time(tm_year=2017, tm_mon=7, tm_mday=14, tm_hour=10, tm_min=40, tm_sec=0, tm_wday=4, tm_yday=195, tm_isdst=0)
#结构化时间-->时间戳　
#time.mktime(结构化时间)
>>>time_tuple = time.localtime(1500000000)
>>>time.mktime(time_tuple)
1500000000.0

 
#结构化时间-->字符串时间
#time.strftime("格式定义","结构化时间")  结构化时间参数若不传，则显示当前时间
>>>time.strftime("%Y-%m-%d %X")
'2017-07-24 14:55:36'
>>>time.strftime("%Y-%m-%d",time.localtime(1500000000))
'2017-07-14'


#字符串时间-->结构化时间
#time.strptime(时间字符串,字符串对应格式)
>>>time.strptime("2017-03-16","%Y-%m-%d")
time.struct_time(tm_year=2017, tm_mon=3, tm_mday=16, tm_hour=0, tm_min=0, tm_sec=0, tm_wday=3, tm_yday=75, tm_isdst=-1)
>>>time.strptime("07/24/2017","%m/%d/%Y")
time.struct_time(tm_year=2017, tm_mon=7, tm_mday=24, tm_hour=0, tm_min=0, tm_sec=0, tm_wday=0, tm_yday=205, tm_isdst=-1)
 
```

![img](http://crm.pythonav.com/media/uploads/2019/04/16/IMAGE-20190416181546-16.GIF)

![img](http://crm.pythonav.com/media/uploads/2019/04/17/IMAGE_bytTldr.PNG)

```python
#结构化时间 --> %a %b %d %H:%M:%S %Y串
#time.asctime(结构化时间) 如果不传参数，直接返回当前时间的格式化串
>>>time.asctime(time.localtime(1500000000))
'Fri Jul 14 10:40:00 2017'
>>>time.asctime()
'Mon Jul 24 15:18:33 2017'



#时间戳 --> %a %b %d %H:%M:%S %Y串
#time.ctime(时间戳)  如果不传参数，直接返回当前时间的格式化串
>>>time.ctime()
'Mon Jul 24 15:19:07 2017'
>>>time.ctime(1500000000)
'Fri Jul 14 10:40:00 2017'
```

**让同学们练习一个计算时间差的代码：**

```python
计算时间差:

import time
true_time=time.mktime(time.strptime('2017-09-11 08:30:00','%Y-%m-%d %H:%M:%S'))
time_now=time.mktime(time.strptime('2017-09-12 11:00:00','%Y-%m-%d %H:%M:%S'))
dif_time=time_now-true_time
struct_time=time.gmtime(dif_time)
print('过去了%d年%d月%d天%d小时%d分钟%d秒'%(struct_time.tm_year-1970,struct_time.tm_mon-1,
                                       struct_time.tm_mday-1,struct_time.tm_hour,
                                       struct_time.tm_min,struct_time.tm_sec))
```

我们看完了time在来看一个Python处理日期和时间的标准库

